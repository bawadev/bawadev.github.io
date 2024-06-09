---
layout:     post
title:      "Introduction to Docker"
subtitle:   "A zero to hero docker guide for java developers"
date:       2024-01-09
author:     "Pasindu"
image:      "https://www.docker.com/wp-content/uploads/2022/12/Docker-Temporary-Image-Social-Thumbnail-1200x630-1.png"
summary: "Docker is a platform designed to help developers build, ship, and run applications using containerization. It has revolutionized the way applications are deployed and managed, making the process more efficient and reliable. This article will provide an in-depth look at Docker, its benefits, installation, and practical usage."
tags:
    - Java
    - Docker
    - CI/CD   
---


Docker is a platform designed to help developers build, ship, and run applications using containerization. It has revolutionized the way applications are deployed and managed, making the process more efficient and reliable. This article will provide an in-depth look at Docker, its benefits, installation, and practical usage.

## Why Docker?

### Compatibility

One of the major challenges in software development is ensuring that an application runs smoothly across different environments. Docker addresses this issue by packaging the application and all its dependencies into a single container. This container can be run on any system that supports Docker, ensuring consistent behavior regardless of the underlying environment. This eliminates the infamous "it works on my machine" problem, significantly enhancing compatibility and reliability.

### Reduced Setup Time

Setting up development environments can be time-consuming and error-prone. Docker simplifies this process by allowing you to define the entire environment in a `Dockerfile`. This file specifies all the dependencies, configurations, and setup commands needed to run the application. With Docker, you can spin up a complete environment in a matter of seconds, drastically reducing setup time and minimizing errors.

### Separate Environments

Docker makes it easy to create and manage separate environments for development, testing, and production. Each environment can be encapsulated in its own container, ensuring that changes in one environment do not affect others. This isolation helps maintain consistency and stability across different stages of the development lifecycle.

## How Docker Operates on a PC and Utilizes Resources

Docker operates by creating lightweight, isolated containers that share the host operating system's kernel. This approach allows multiple containers to run on a single physical machine with minimal overhead. Docker utilizes a client-server architecture, where the Docker client communicates with the Docker daemon (server) to build, run, and manage containers.

### Resource Utilization and Performance

Containers are more efficient than traditional virtual machines because they share the host OS kernel and do not require a full guest OS per instance. This reduces the amount of resources needed and allows for faster startup times. Docker also provides tools to monitor and manage resource usage, ensuring that containers do not consume more than their allocated share of CPU, memory, and storage.

## Key Docker Terminology

![alt text](https://media.dev.to/cdn-cgi/image/width=1600,height=900,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Frdlipe7r7g8fenj7ahg4.png)

### Dockerfile

A `Dockerfile` is a script that contains a series of instructions on how to build a Docker image. It specifies the base image, environment variables, commands to run, and other necessary configurations.

### Docker Daemon

The Docker daemon (`dockerd`) is a background service responsible for managing Docker containers, images, networks, and storage volumes. It listens for Docker API requests and handles container operations.

### Docker Image

A Docker image is a read-only template used to create Docker containers. Images are built from Dockerfiles and can be stored in registries like Docker Hub.

### Docker Container

A Docker container is a runnable instance of a Docker image. Containers can be started, stopped, moved, and deleted. Each container is isolated from others, providing a secure environment for applications.

![alt text](https://lemariva.com/storage/app/media/blog_imgs/analytics/docker/docker_architecture.svg)

## How to Install Docker


### Installing Docker on Ubuntu

If you are trying docker on windows installing docker on windows is very easy with docker desktop. 
Just go to the official site on docker installing (https://docs.docker.com/engine/install/) and follow along the instructions.

### Installing Docker on Ubuntu

If you are trying to setup docker on your linux server you can quickly start with following commands

1. **Update your package index:**
    ```bash
    sudo apt-get update
    ```

2. **Install necessary packages:**
    ```bash
    sudo apt-get install \
        ca-certificates \
        curl \
        gnupg \
        lsb-release
    ```

3. **Add Docker's official GPG key:**
    ```bash
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    ```

4. **Set up the stable repository:**
    ```bash
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```

5. **Install Docker Engine:**
    ```bash
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io
    ```

6. **Verify the installation:**
    ```bash
    sudo docker run hello-world
    ```

## How to Dockerize an Application

## Dockerizing an Application (in short)

**1. Create application:**
    I think you know how 😍

**2. Create a Dockerfile:** 
   - Base image (e.g., Ubuntu)
   - Install dependencies
   - Copy application code
   - Set environment variables
   - Expose ports
   - Define startup command

**3. Build the Image:**
 `docker build` command

**4. Run the Container:** 
`docker run` command

### Dockerizing a Simple Spring Boot Application
Here we'll try to dockerize a simple hellow world application with spring boot. But you can dockerize anyting even the moon if your machine is big enough 😂

#### Create a Simple Spring boot application
You can create your own spring boot hellow world application by your self. If you are reading this I know you can. To make that easier I created a simple application repo. you cand just clone and start from there too. ( https://github.com/bawadev/spring-boot-docker/ )

#### Create a Dockerfile

1. **Create a file named `Dockerfile` :**
 In the root directory of your Spring Boot project create a file named Dockerfile with the following content
    ```dockerfile
    FROM openjdk:8
    EXPOSE 8080
    ADD target/spring-boot-docker.jar spring-boot-docker.jar
    ENTRYPOINT ["java","-jar","/spring-boot-docker.jar"]
    ```
    - `FROM openjdk:8` : This line tells Docker to use the official OpenJdk8.
    - `EXPOSE 8080` : This line tells Docker that the container listens on port
    - `ADD target/spring-boot-docker.jar spring-boot-docker.jar` : This line tells docker to get the artifact named spring-boot-docker.jar from target
    - `ENTRYPOINT ["java","-jar","/spring-boot-docker.jar"]` : This instructs docker to how to form the run the jar file inside the container.

    Docker file is an essentially like a instruction to wrap an image with other dependencies. In here it says lay a jdk first, then put jar on top of that. Like wise you can add dependencies with their versions as layers inside an image.

2. **Build the Docker image:**
Open an terminal at the root of the project and run 
    ```bash
    docker build -t spring-boot-docker .
    ```

3. **Run the Docker container:**
then run
    ```bash
    docker run -p 8080:8080 spring-boot-docker
    ```
In here 8080:8080 what you are doing is mapping the container port with your computer port. Then when ever a request to 8080 in your computer will be handled by the container application 

4. **Test the application using `curl`:**
    ```bash
    curl http://localhost:8080
    ```

## Useful Docker Commands

Finally let me put some usefull docker commands for you to memorize and flex on other people who don't konw docker 😂

### `docker run`

Runs a command in a new container.
```bash
docker run -p 8080:8080 spring-boot-docker
```
- `-p 8080:8080`: Maps port 8080 of the container to port 8080 on the host.

### `docker ps`

Lists running containers.
```bash
docker ps
```

### `docker ps -a`

Lists all containers, including stopped ones.
```bash
docker ps -a
```

### `docker rmi`

Removes one or more Docker images.
```bash
docker rmi spring-boot-docker
```

### `docker pull`

Pulls an image from a Docker registry.
```bash
docker pull ubuntu
```

### `docker images`

Lists all Docker images on the host.
```bash
docker images
```

### `docker exec`

Runs a command in a running container.
```bash
docker exec -it <container_id> /bin/bash
```

By understanding and utilizing these commands, you can effectively manage Docker containers and images.

## Conclusion

Docker provides a powerful and efficient way to develop, deploy, and manage applications. Its ability to ensure compatibility, reduce setup times, and maintain separate environments makes it an invaluable tool for developers. By following the steps outlined in this article, you can start using Docker to streamline your development workflow and improve the reliability of your applications.
