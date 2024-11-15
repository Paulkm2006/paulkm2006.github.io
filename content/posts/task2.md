+++ 
draft = false
date = 2024-11-14T21:33:43+08:00
title = "Task2 notes"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++

## Dockerfile

A Dockerfile is used to build a docker container. We can specify the container's building command, running command and resources in it.

### Multistage build

We can split the building process of a complete image into small stages. E.g. create a 'builder' stage to compile your application, and then specify its startup parameters in the main stage.

### Ports vs export

When a pair of ports `host_port:image_port` is specified, docker will forward the port onto the host's network interface, allowing external access.
In contrast, when a port is specified using expose, it will only be available in the local machine's docker networks.

For services that doesn't require external access (like databases, message queues), it's a good idea to keep their access within the local machine. 

### How containers communicate with each other

#### Bridged network
When a container's network type is not specified, Docker will connect the container using the default bridge network. This allows containers to communicate with each other using their container names as hostnames.

We can also create our own bridge network to control which instances can connect to who.

#### Overlay network

In Docker Swarm clusters, we can put containers into a overlay network, allowing them to communicate with each other, regardless whether they are on the same host or not.

## Docker Compose

For applications that requires external services like databases, we can integrate the required service in the built docker image. However, doing so will cause the size of the image to increase significantly, and also making it hard to update or config the services.

Luckily, Docker compose is here to help! By writing a `docker-compose.yml` file, we can create and manage multiple images together.

A typical `docker-compose.yml` looks like this:
```yml
services:
  nginx: # name of the service
    image: nginx:latest # Original image
    container_name: nginx # container name, will be used as hostname for inter-container access
    ports:
      - 80:80 # Map port 80 on the host to port 80 on the container
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf # Mount the local nginx.conf file to the container
    depends_on:
      - hello # Wait for the hello service to be ready before starting the nginx service
  hello:
    build: . # Build the image from the Dockerfile in the current directory
    container_name: hello
    expose:
      - 8080 # Expose port 8080 on the container, allowing other services to access it
	develop:
      watch: # watch file changes
        - action: sync # sync the file if it's changed outside the container
          path: ./web
          target: /src/web
          ignore:
            - node_modules/
        - action: rebuild # rebuild the container on changes
          path: package.json
```