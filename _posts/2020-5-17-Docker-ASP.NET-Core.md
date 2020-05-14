---
layout: post
title:  "Docker images for ASP.NET Core"
categories: docker
tags:   azure kubernetes docker helm
---

This [tutorial](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/docker/building-net-docker-images?view=aspnetcore-3.1) shows how to run an ASP.NET Core app in Docker containers.

In this tutorial, you:

- Learn about Microsoft .NET Core Docker images
- Download an ASP.NET Core sample app
- Run the sample app locally
- Run the sample app in Linux containers
- Run the sample app in Windows containers
- Build and deploy manually

Dockerfile:

```yaml
FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build
WORKDIR /app

# copy csproj and restore as distinct layers
# COPY *.sln .
COPY *.csproj .
RUN dotnet restore

# copy everything else and build app
COPY . ./aspnetapp/
WORKDIR /app/aspnetapp
RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/core/aspnet:2.2 AS runtime
WORKDIR /app
COPY --from=build /app/aspnetapp/out ./
ENTRYPOINT ["dotnet", "mywebapi.dll"]
```

The sample Dockerfile uses the Docker multi-stage build feature to build and run in different containers. The build and run containers are created from images that are provided in Docker Hub by Microsoft:

>dotnet/core/sdk

The sample uses this image for building the app. The image contains the .NET Core SDK, which includes the Command Line Tools (CLI). The image is optimized for local development, debugging, and unit testing. The tools installed for development and compilation make this a relatively large image.

>dotnet/core/aspnet

The sample uses this image for running the app. The image contains the ASP.NET Core runtime and libraries and is optimized for running apps in production. Designed for speed of deployment and app startup, the image is relatively small, so network performance from Docker Registry to Docker host is optimized. Only the binaries and content needed to run an app are copied to the container. The contents are ready to run, enabling the fastest time from Docker run to app startup. Dynamic code compilation isn't needed in the Docker model.

### Run the app locally

```bash
# Build image
docker build -t mywebapi:latest .
# Run
docker run mywebapi
```

### Push the image to the ACR

```bash
docker tag mywebapi loyaltycloud.azurecr.io/mywebapi:v2
docker push loyaltycloud.azurecr.io/mywebapi:v2
# Verify
az acr repository list --name loyaltycloud --output table
az acr repository show  -n loyaltycloud --repository mywebapi
{
  "changeableAttributes": {
    "deleteEnabled": true,
    "listEnabled": true,
    "readEnabled": true,
    "writeEnabled": true
  },
  "createdTime": "2020-05-14T10:11:06.3620347Z",
  "imageName": "mywebapi",
  "lastUpdateTime": "2020-05-14T11:59:35.8228843Z",
  "manifestCount": 2,
  "registry": "loyaltycloud.azurecr.io",
  "tagCount": 2
}

az acr repository show-tags  -n loyaltycloud --repository mywebapi
[
  "v1",
  "v2"
]
```

### Adapt your Helm chart

Update appVersion to v2 in mywebapi/Chart.yaml. For example:

```yaml
apiVersion: v2
name: mywebapi
...
# This is the version number of the application being deployed. This version number should be
# incremented each time you make changes to the application.
appVersion: v2
```

### Run your Helm chart

```bash
helm install mywebapi mywebapi/ -n sfan
NAME: mywebapi
LAST DEPLOYED: Thu May 14 14:08:04 2020
NAMESPACE: sfan
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
     NOTE: It may take a few minutes for the LoadBalancer IP to be available.
           You can watch the status of by running 'kubectl get --namespace sfan svc -w mywebapi'
  export SERVICE_IP=$(kubectl get svc --namespace sfan mywebapi --template "{{ range (index .status.loadBalancer.ingress 0) }}{{.}}{{ end }}")
  echo http://$SERVICE_IP:80
# Verify
kubectl -n sfan get services
NAME       TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)        AGE
mywebapi   LoadBalancer   10.0.253.139   51.105.250.176   80:32301/TCP   40s
```

### Trouble shoot

```bash
kubectl -n sfan get pods
NAME                        READY   STATUS             RESTARTS   AGE
mywebapi-554496fcc8-vgbw4   0/1     CrashLoopBackOff   6          6m17s
kubectl -n sfan describe pod

```

### Upgrade applications with helm

```bash
# Current status
helm -n sfan list
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
mywebapi        sfan            1               2020-05-14 14:08:04.86447333 +0200 CEST deployed        mywebapi-0.1.0  v2

# perform upgrading
helm -n sfan upgrade mywebapi ./mywebapi
Release "mywebapi" has been upgraded. Happy Helming!
NAME: mywebapi
LAST DEPLOYED: Thu May 14 14:44:43 2020
NAMESPACE: sfan
STATUS: deployed
REVISION: 2

# Verify
helm -n sfan list
NAME            NAMESPACE       REVISION        UPDATED                                         STATUS          CHART           APP VERSION
mywebapi        sfan            2               2020-05-14 14:44:43.143223606 +0200 CEST        deployed        mywebapi-0.1.0  v3

kubectl -n sfan get services
NAME       TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)        AGE
mywebapi   LoadBalancer   10.0.253.139   51.105.250.176   80:32301/TCP   37m

kubectl -n sfan get pods
NAME                        READY   STATUS    RESTARTS   AGE
mywebapi-7d6c48f977-6lf5g   1/1     Running   0          49s
```

### Uninstall helm chart

```bash
helm -n sfan uninstall mywebapi
helm -n sfan list
```
