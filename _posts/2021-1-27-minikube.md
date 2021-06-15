---
layout: post
title:  "Minikube"
categories: k8s
tags:   k8s minikube
---

## Installation

```
https://minikube.sigs.k8s.io/docs/start/
```
## Start your cluster

```
minikube start
```

## Dashboard

```
minikube dashboard
```

## Switch kubectl context

```bash
$ kubectl config get-contexts
CURRENT   NAME                        CLUSTER                     AUTHINFO                                                          NAMESPACE
          loyalty-cloud-development   loyalty-cloud-development   clusterUser_loyalty-cloud-development_loyalty-cloud-development   
*         minikube                    minikube                    minikube                                                          default
$ kubectl config use minikube
Switched to context "minikube".
```


## Reference

[minikube get started](https://minikube.sigs.k8s.io/docs/start/)