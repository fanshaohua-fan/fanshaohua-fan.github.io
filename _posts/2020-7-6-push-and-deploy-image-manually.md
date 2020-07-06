---
layout: post
title:  "Push and Deploy your image manually"
categories: kubernetes
tags:   acr kubernetes docker
---

## Push your image manually to ACR

### Check the current version in ACR

```sh
az acr repository list --name loyaltycloud --output table
az acr repository show-tags  -n loyaltycloud --repository strapi-app
az acr repository show-manifests  -n loyaltycloud --repository strapi-app
```

### Check your local image

```sh
docker images | grep strapi-app
loyalty-cloud_strapi-app                        latest                   8deb48164f6e        7 minutes ago       1.11GB
loyaltycloud.azurecr.io/strapi-app              latest                   5e3e888453c5        5 weeks ago         1.08GB
```

### tag your local image

```sh
docker tag loyalty-cloud_strapi-app loyaltycloud.azurecr.io/strapi-app:latest

docker images | grep strapi-app
loyalty-cloud_strapi-app                        latest                   8deb48164f6e        10 minutes ago      1.11GB
loyaltycloud.azurecr.io/strapi-app              latest                   8deb48164f6e        10 minutes ago      1.11GB
loyaltycloud.azurecr.io/strapi-app              <none>                   5e3e888453c5        5 weeks ago         1.08GB

```

### Push the image to ACR

Before pushing and pulling container images, you must log in to the registry. To do so, use the `az acr login` command.

```sh
az acr login --name loyaltycloud
Login Succeeded

docker push loyaltycloud.azurecr.io/strapi-app:latest
latest: digest: sha256:2ace75c4646effab2a7a8998447843bb55a6eac6be169120ed1a5b464c1b15b2 size: 2631

az acr repository show-manifests  -n loyaltycloud --repository strapi-app --output table
Digest                                                                   Timestamp
-----------------------------------------------------------------------  ----------------------------
sha256:0d923be872f881153ff09d05b3f47ca43d0f4896110c52d7479182006a5d33dc  2020-05-15T10:20:26.7823774Z
sha256:12906d9f0815951568889b1f269f8f95a5ec11d7c73674f55014a717aa03463c  2020-05-25T13:10:19.9800842Z
sha256:2ace75c4646effab2a7a8998447843bb55a6eac6be169120ed1a5b464c1b15b2  2020-07-06T08:14:36.1096696Z
sha256:7f39e593443a2e17078142bdbc6ce585d585b9ff1bd0bda8d9371f7b5099388b  2020-05-18T12:31:43.1375896Z
sha256:7f9eb131194f3d8b4a5eaeb6e4ac087060685da2490fc80d41f948fec8459632  2020-05-22T15:09:50.0201538Z
sha256:cbcd602bd7f70cbbb3637ae10c980b958b9ecd9fc0da206b9283c768a62ecc82  2020-05-18T13:23:26.2808536Z
sha256:de5bae27c9c5c2775082da0e618bc0a736a9a5fa8e5a3ccd493cf3a132c54e60  2020-05-14T17:29:12.2433332Z
sha256:debb0c63215fc02846b7ae125d331922602bd70edb66b7ec7664d15459f63abe  2020-05-15T12:37:04.7237523Z
sha256:e5608f8044ab105652acc888266a7417a901862b2715e34ebd9ea389c3e954a8  2020-05-15T09:43:46.8958075Z
sha256:ea85f0875efc26fb28912eebd4e275257e4075eae77c7f41a0599dbac136ab87  2020-05-14T18:00:17.9285214Z
```

### Deploy to Azure Kubernetes Service (AKS) cluster

```bash
# Check the current deployed version
kubectl -n lc-demo get pods | grep strapi-app
strapi-app-v1-98dc449c-9p5wq                2/2     Running   1          44d

kubectl -n lc-demo describe pods/strapi-app-v1-98dc449c-9p5wq
Containers:
  strapi-app:
    Container ID:   docker://2ec29dc1a2913397f010179006731e8035dd9bbe6a400775eefe34c68148d41e
    Image:          loyaltycloud.azurecr.io/strapi-app:latest
    Image ID:       docker-pullable://loyaltycloud.azurecr.io/strapi-app@sha256:cbcd602bd7f70cbbb3637ae10c980b958b9ecd9fc0da206b9283c768a62ecc82


# Update your app manually via kubectl
kubectl -n lc-demo set image deployments/strapi-app-v1 strapi-app=loyaltycloud.azurecr.io/strapi-app@sha256:2ace75c4646effab2a7a8998447843bb55a6eac6be169120ed1a5b464c1b15b2
deployment.extensions/strapi-app-v1 image updated

# Check the rollout status
kubectl -n lc-demo rollout status deployments/strapi-app-v1
Waiting for deployment "strapi-app-v1" rollout to finish: 1 old replicas are pending termination...

kubectl -n lc-demo get pods | grep strapi
strapi-app-v1-7c7ffbbd64-6htv6              0/2     PodInitializing   0          56s
strapi-app-v1-98dc449c-9p5wq                2/2     Running           1          44d

kubectl -n lc-demo rollout status deployments/strapi-app-v1
deployment "strapi-app-v1" successfully rolled out

kubectl -n lc-demo get pods | grep strapi
strapi-app-v1-7c7ffbbd64-6htv6              2/2     Running   1          2m10s

kubectl -n lc-demo rollout history deployments/strapi-app-v1
deployment.extensions/strapi-app-v1
REVISION  CHANGE-CAUSE
1         <none>
2         <none>
3         <none>
```
