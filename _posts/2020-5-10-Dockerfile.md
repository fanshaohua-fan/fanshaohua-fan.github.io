---
layout: post
title:  "Best practices for Dockerfile"
categories: docker
tags:   docker dockerfile
---

Best practices for writing Dockerfiles
-----

Run arbitrary commands inside an existing container

```bash
docker exec -it <mycontainer> bash
```


[how to run docker exec on a docker-compose.yml](https://stackoverflow.com/questions/45774221/how-to-run-docker-exec-on-a-docker-compose-yml)