---
layout: post
title:  "Kubernetes Learning Path"
categories: kubernetes
tags:   container azure kubernetes
---

[50 days from zero to hero with Kubernetes](https://azure.microsoft.com/mediahandler/files/resourcefiles/kubernetes-learning-path/Kubernetes%20Learning%20Path%20version%201.0.pdf)

![from zero to hero](/images/2020-05-12-09-33-13.png)


## Day 1

- Pods
- ReplicaSets
- Secrets
- Deployments
- DaemonSets
- Ingresses
- CronJobs
- CRDs(Custom Resource Definitions)

## Day 2 Kubernetes basics

[video series with Brendan Burns](https://www.youtube.com/playlist?list=PLLasX02E8BPCrIhFrc_ZiINhbRkYMKdPT)


### Scheduler

- Predicates
  - Memory
  - Node selector
- Priorities
  - Spreading
  - Prefer

### K8s build pipeline

- Admission controller
  - image: myreg.acr.io/*

Git -> Build pipeline -> ACR -> K8s clusters

### How volumes and storage works

- empty Dir
- hostPath

- persistent volume
  - persistent volume claims

### Stateful applications in K8s

### Secrets management

- password to the database
- certificate

Secret is a collection of key/value pairs, stored in etcd.

## Problem solving

### Make changes on certain resource

```bash
kg get job analytics-calculate-dashboard-1635811200 -o json

kg edit job analytics-calculate-dashboard-1635811200 -o json
```