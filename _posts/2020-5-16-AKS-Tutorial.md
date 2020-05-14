---
layout: post
title:  "Tutorial: Azure Kubernetes Service(AKS)"
categories: kubernetes
tags:   azure kubernetes
---

[AKS Tutorial](https://docs.microsoft.com/en-us/azure/aks/tutorial-kubernetes-prepare-app)

### Prepare an application for Azure Kubernetes Service

Docker Compose can be used to automate building container images and the deployment of multi-container applications.

Use the sample docker-compose.yaml file to create the container image, download the Redis image, and start the application:

```bash
docker-compose up -d

docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                           NAMES
f31d08a79653        azure-vote-front    "/entrypoint.sh /sta…"   About a minute ago   Up About a minute   443/tcp, 0.0.0.0:8080->80/tcp   azure-vote-front
375538721eaf        redis               "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:6379->6379/tcp          azure-vote-back
```

Stop and remove the container instances and resources with the docker-compose down command:

```bash
docker-compose down
```

When the local application has been removed, you have a Docker image that contains the Azure Vote application, azure-vote-front, for use with the next tutorial.

### Deploy and use Azure Container Registry

Learning objective:

- Create an Azure Container Registry (ACR) instance
- Tag a container image for ACR
- Upload the image to ACR
- View images in your registry

Azure CLI change current subscription

```bash
az account set -h

Command
    az account set : Set a subscription to be the current active subscription.

Arguments
    --subscription -s [Required] : Name or ID of subscription.

az account set -s 91067109-d02e-4436-b81d-17500d1015de
```


```bash
# add yourself to the docker group, then restart your server/pc.
sudo usermod -a -G docker your-user-name

az acr list
# sudo az acr login --name loyaltycloud
az acr login --name loyaltycloud

# Tag a container image
sudo docker tag azure-vote-front loyaltycloud.azurecr.io/azure-vote-front:v1

# Push images to registry
docker push loyaltycloud.azurecr.io/azure-vote-front:v1

az acr repository list --name loyaltycloud --output table
az acr repository show  -n loyaltycloud --repository azure-vote-front
az acr repository show-tags  -n loyaltycloud --repository azure-vote-front
```

### Deploy an Azure Kubernetes Service (AKS) cluster

You learn how to:

- Deploy a Kubernetes AKS cluster that can authenticate to an Azure container registry
- Install the Kubernetes CLI (kubectl)
- Configure kubectl to connect to your AKS cluster

```bash
# Install the Kubernetes CLI
sudo az aks install-cli

# List all the aks
az aks list -g loyalty-cloud-testing-resourcegroup

# Connect to cluster using kubectl
az aks get-credentials --resource-group loyalty-cloud-testing-resourcegroup --name loyalty-cloud-testing-cluster

# List the cluster nodes
kubectl get nodes
```

### Run applications in Azure Kubernetes Service (AKS)

You learn how to:

- Update a Kubernetes manifest file
- Run an application in Kubernetes
- Test the application

```bash
# Update the manifest file
vi azure-vote-all-in-one-redis.yaml
# Replace microsoft with your ACR login server name

# Deploy the application
kubectl apply -f azure-vote-all-in-one-redis.yaml

# Test the application
kubectl get service azure-vote-front --watch

kubectl get nodes
kubectl get services
kubectl get replicaset
kubectl get deployment
kubectl get pods
kubectl get secret
kubectl get daemonset
kubectl get ingress
kubectl get cronjob

# list all the resources from all namespaces
kubectl get services -a

# list all the resources under a certain namespace
kubectl -n demo get services
kubectl -n demo get nodes
kubectl -n demo get pods
kubectl -n demo get deployment
kubectl -n demo get secret
kubectl -n demo get replicaset
kubectl -n demo get daemonset
kubectl -n demo get ingress
kubectl -n demo get cronjob

```

### Scale applications

You learn how to:

- Scale the Kubernetes nodes
- Manually scale Kubernetes pods that run your application
- Configure autoscaling pods that run the app front-end

**Manually scale pods**

```bash
kubectl get pods
kubectl scale --replicas=3 deployment/azure-vote-front

```

**Autoscale pods**

To use the autoscaler, all containers in your pods and your pods must have CPU requests and limits defined. In the azure-vote-front deployment, the front-end container already requests 0.25 CPU, with a limit of 0.5 CPU. These resource requests and limits are defined as shown in the following example snippet:

```yaml
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

The following example uses the kubectl autoscale command to autoscale the number of pods in the azure-vote-front deployment. If average CPU utilization across all pods exceeds 50% of their requested usage, the autoscaler increases the pods up to a maximum of 10 instances. A minimum of 3 instances is then defined for the deployment:

```bash
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

### Update an application

You learn how to:

- Update the front-end application code
- Create an updated container image
- Push the container image to Azure Container Registry
- Deploy the updated container image

```bash
# Update the container image
docker-compose up --build -d

# Tag and push the image
docker tag azure-vote-front loyaltycloud.azurecr.io/azure-vote-front:v2
docker push loyaltycloud.azurecr.io/azure-vote-front:v2

# Deploy the updated application
kubectl get pods

kubectl set image deployment azure-vote-front azure-vote-front=loyaltycloud.azurecr.io/azure-vote-front:v2

# Verify
kubectl get pods
NAME                                READY   STATUS              RESTARTS   AGE
azure-vote-back-5775d78ff5-98rls    1/1     Running             0          16h
azure-vote-front-58869f478c-db5xw   1/1     Running             0          13s
azure-vote-front-58869f478c-f2tzt   0/1     ContainerCreating   0          4s
azure-vote-front-58869f478c-p65vl   1/1     Running             0          13s
azure-vote-front-5b4f7cc4f8-9cml7   0/1     Terminating         0          16h
azure-vote-front-5b4f7cc4f8-bgdhb   1/1     Terminating         0          16h

kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
azure-vote-back-5775d78ff5-98rls    1/1     Running   0          16h
azure-vote-front-58869f478c-db5xw   1/1     Running   0          51s
azure-vote-front-58869f478c-f2tzt   1/1     Running   0          42s
azure-vote-front-58869f478c-p65vl   1/1     Running   0          51s

kubectl get replicaset
NAME                          DESIRED   CURRENT   READY   AGE
azure-vote-back-5775d78ff5    1         1         1       16h
azure-vote-front-58869f478c   3         3         3       2m27s
azure-vote-front-5b4f7cc4f8   0         0         0       16h
```

### Upgrade Kubernetes cluster

As part of the application and cluster lifecycle, you may wish to upgrade to the latest available version of Kubernetes and use new features. An Azure Kubernetes Service (AKS) cluster can be upgraded using the Azure CLI.

In this tutorial, part seven of seven, a Kubernetes cluster is upgraded. You learn how to:

- Identify current and available Kubernetes versions
- Upgrade the Kubernetes nodes
- Validate a successful upgrade

```bash
# Get available cluster versions
az aks get-upgrades --resource-group loyalty-cloud-testing-resourcegroup --name loyalty-cloud-testing-cluster --output=table

# Upgrade
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.16.5

# Verify
az aks show --resource-group loyalty-cloud-testing-resourcegroup --name loyalty-cloud-testing-cluster --output table
```
