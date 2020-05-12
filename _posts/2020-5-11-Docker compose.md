---
layout: post
title:  "Docker Compose"
categories: docker
tags:   docker-compose
---

Docker Compose

Compose has commands for managing the whole lifecycle of your application:

- Start, stop, and rebuild services
- View the status of running services
- Stream the log output of running services
- Run a one-off command on a service

## Deploying changes

When you make changes to your app code, remember to rebuild your image and recreate your app’s containers. To redeploy a service called **web**, use:

```bash
docker-compose build web
docker-compose up --no-deps -d web


$ sudo docker.compose build strapi
Building strapi
Step 1/2 : FROM strapi/strapi:3.0.0-beta.19.3-alpine
 ---> 25c6ef17defb
Step 2/2 : COPY ./app /srv/app
 ---> e798677fed49
Successfully built e798677fed49
Successfully tagged strapi3_strapi:latest

$ sudo docker.compose images
   Container       Repository    Tag       Image Id      Size 
--------------------------------------------------------------
strapi3_strapi_1   <none>       <none>   87fc37c874d5   368 MB
$ sudo docker.compose up --no-deps -d strapi
Recreating strapi3_strapi_1 ... done
$ sudo docker.compose ps
      Name                    Command               State           Ports
----------------------------------------------------------------------------------
strapi3_strapi_1   docker-entrypoint.sh strap ...   Up      0.0.0.0:9080->1337/tcp
$ sudo docker.compose images
   Container         Repository      Tag       Image Id      Size 
------------------------------------------------------------------
strapi3_strapi_1   strapi3_strapi   latest   e798677fed49   368 MB
$
```

This first rebuilds the image for web and then stop, destroy, and recreate just the web service. The --no-deps flag prevents Compose from also recreating any services which **web** depends on.

## Control startup order

[Control startup and shutdown order in Compose](https://docs.docker.com/compose/startup-order/)

 However, for startup Compose does not wait until a container is “ready” (whatever that means for your particular application) - only until it’s running. There’s a good reason for this.

The problem of waiting for a database (for example) to be ready is really just a subset of a much larger problem of distributed systems. In production, your database could become unavailable or move hosts at any time. Your application needs to be resilient to these types of failures.

To handle this, design your application to attempt to re-establish a connection to the database after a failure. If the application retries the connection, it can eventually connect to the database.

The best solution is to perform this check in your application code, both at startup and whenever a connection is lost for any reason. However, if you don’t need this level of resilience, you can work around the problem with a wrapper script:

- Use a tool such as wait-for-it, dockerize, or sh-compatible wait-for. These are small wrapper scripts which you can include in your application’s image to poll a given host and port until it’s accepting TCP connections.
 For example, to use wait-for-it.sh or wait-for to wrap your service’s command:

```yaml
version: "2"
services:
  web:
    build: .
    ports:
      - "80:8000"
    depends_on:
      - "db"
    command: ["./wait-for-it.sh", "db:5432", "--", "python", "app.py"]
  db:
    image: postgres
```
