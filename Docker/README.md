# Docker

![Docker](Docker-logo.png)


# What is Docker?

Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries, configurations and other dependencies, and deploy it as one package.

# Why Docker?

Docker solves the problem of "It is running on my machine". Docker allows you to run your application in the same environment everywhere. It is a tool that allows you to deploy your application in a container. If the application works on your machine, it will work on any machine (containing the production server)

Docker also help the developers work on different projects with different dependencies on the same machine without any conflicts  or the hassle of installing and uninstalling dependencies.

# Containers vs Virtual Machines

Containers and virtual machines have similar resource isolation and allocation benefits, but function differently because containers virtualize the operating system instead of hardware. Containers are more portable and efficient.

In short, virtual machines have their own OS and containers share the OS of the host machine.

# Image vs Container

An image is a template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.

Containers are a running instance of an image. You can see a list of your running containers with the command, docker ps, just as you would in Linux.

In short, containers are running environments of images.

# Docker Hub

Docker Hub is a cloud-based registry service which allows you to link to code repositories, build your images and test them, stores manually pushed images, and links to Docker Cloud so you can deploy images to your hosts. It provides a centralized resource for container image discovery, distribution and change management, user and team collaboration, and workflow automation throughout the development pipeline.

# Docker Installation

You can use google to find the installation steps for your OS. To make sure that docker is installed correctly, run the following command in your terminal:

```bash
$ docker version
```

This will display the version of docker client and server.

> If the version information of the docker client is only displayed, make sure that the docker engine is running. (Docker Desktop on Windows, Mac and linux starts the docker engine automatically)


# Development Workflow

The development workflow is as follows:

Create a Dockerfile inside your project directory, Dockerfile is a text file that contains all the commands a user could call on the command line to assemble an image.


The image contains all the dependencies and configuration required to run your application:

- A cut-down operating system (e.g. Alpine Linux)
- Your application and its dependencies (e.g. Python, Node.js, Java)
- Application files
- Third party libraries
- Environment variables

Once we have an image, we can push it to docker hub (a registry for docker images), docker hub for docker is like github for git. We can also pull images from docker hub.

# Hello World

Let's pull and run the most basic docker image:

```bash
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
719385e32844: Pull complete
Digest: sha256:4f53e2564790c8e7856ec08e384732aa38dc43c52f02952483e3f003afbf23db
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.    

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.     
    (amd64)
 3. The Docker daemon created a new container from that image which runs the  
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:        
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

```

the command will check the local machine for the image, if it doesn't find it, it will pull it from docker hub and then run it.



# Docker in Action

Let's build a simple node.js application and dockerize it.

## Step 1: Create a node.js application

Create a directory for your project and cd into it:

```bash
$ mkdir node-app
$ cd node-app
```

Create a file named `index.js` and add the following code to it:

```js
console.log("Hello World");
```

Create a file named `package.json` and add the following code to it:

```json
{
  "name": "node-app",
  "version": "1.0.0",
  "description": "A simple node.js application",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  }
}
```

To test your application, run the following command:

```bash
$ npm start
```

You should see the following output:

```bash
$ npm start

> node-app@1.0.0 start
> node index.js

Hello World
```

## Step 2: Create a Dockerfile

Create a file named `Dockerfile` and add the following code to it:

```Dockerfile
FROM node:alpine
COPY . /app
WORKDIR /app
CMD npm start
```

Let's go through the Dockerfile line by line:

- `FROM node:alpine` tells docker to use the node:alpine image from docker hub as the base image for our image. The node:alpine image is a cut-down version of the node image. It is much smaller in size and contains only the minimal packages required to run node.js applications.

- `COPY . /app` copies the current directory (the node-app directory) into the /app directory inside the image.

- `WORKDIR /app` sets the working directory inside the image to /app. This means that all subsequent commands will be run from the /app directory and no need to specify the full path.

- `CMD npm start` tells docker to run the command npm start when the container is started.

## Step 3: Build the image

To build the image, run the following command:

```bash
$ docker build -t node-app .
```

Let's go through the command in detail:

- `docker build` tells docker to build an image.

- `-t node-app` tells docker to tag the image with the name node-app.

- `.` tells docker to look for the Dockerfile in the current directory.

Now run the following command to see the list of images:

```bash
$ docker images
```

## Step 4: Run the image

To run the image, run the following command:

```bash
$ docker run node-app
```

You should see the following output:

```bash
$ docker run node-app

> node-app@1.0.0 start
> node index.js

Hello World
```




# Basic Docker Commands

## pull

To pull an image from docker hub, run the following command:

```bash
$ docker pull <image-name>
```

## run

To run an image, run the following command:

```bash
$ docker run <image-name>
```

## name

Docker by defaults gives a random name to a container.

To name a container, run the following command:

```bash
$ docker run --name <container-name> <image-name>
```

## start

To start a container, run the following command:

```bash
$ docker start <container-name>
```

## stop

To stop a container, run the following command:

```bash
$ docker stop <container-name>
```

## detach mode

Detach mode allows you to run a container in the background. It does not receive input or display output. To run a container in detach mode, run the following command:

```bash
$ docker run -d <image-name>
```




## ps

To see the list of running containers, run the following command:

```bash
$ docker ps
```

To see the list of all containers (running and stopped), run the following command:

```bash
$ docker ps -a
```


## Port binding

Docker port binding allows you to map a container's internal port to a port on the host machine, enabling external access to the containerized application.

To bind a container's internal port to a port on the host machine, run the following command:

```bash
$ docker run -p <host-port>:<container-port> <image-name>
```

## logs

To see the logs of a container, run the following command:

```bash
$ docker logs <container-name>
```

## exec

To run a command inside a container, run the following command:

```bash
$ docker exec <container-name> <command>
```

to run a shell inside a container, run the following command:

```bash
$ docker exec -it <container-name> sh
```

where `-it` stands for interactive terminal.

# Docker Networking

Docker networking allows you to create a network and connect containers to it. Containers connected to the same network can communicate with each other.


## network create

To create a network, run the following command:

```bash
$ docker network create <network-name>
```

## network ls

To see the list of networks, run the following command:

```bash
$ docker network ls
```

## network inspect

To see the details of a network, run the following command:

```bash
$ docker network inspect <network-name>
```

## network connect

To connect a container to a network, run the following command:

```bash
$ docker network connect <network-name> <container-name>
```



# Docker Compose

Docker compose is a tool for defining and running multi-container docker applications. It uses a YAML file to configure the application's services and performs the creation and start-up process of all the containers with a single command.

> Note : Docker compose takes care of creating a network and connecting all the containers to it.

## docker-compose.yml

Create a file named `docker-compose.yml` and add the following code to it:

```yml
version: "3.8"
services:
  mongodb:
      image: mongo
      ports:
         - "27017:27017"
      environment:
         - MONGO_INITDB_ROOT_USERNAME=admin
         - MONGO_INITDB_ROOT_PASSWORD=password
   mongo-express:
      image: mongo-express
      ports:
         - "8081:8081"
      environment:
         - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
         - ME_CONFIG_MONGODB_ADMINPASSWORD=password
         - ME_CONFIG_MONGODB_SERVER=mongodb
```

Son instead of running two containers with the command line, we can run them with a single command based on the configuration in the docker-compose.yml file.

To run the containers, run the following command:

```bash
$ docker-compose up
```

to start the containers from other file than the default `docker-compose.yml` file, run the following command:

```bash
$ docker-compose -f <file-name> up
```

To stop the containers, run the following command:

```bash
$ docker-compose down
```

# Docker file

Dockerfile is a text file that contains all the commands a user could call on the command line to assemble an image.

## FROM

The `FROM` instruction sets the Base Image for subsequent instructions. As such, a valid Dockerfile must have `FROM` as its first instruction. The image can be any valid image – it is especially easy to start by pulling an image from the Public Repositories.

```Dockerfile
FROM <image-name>
```


for example:

```Dockerfile
FROM node:alpine
```

## ENV

The `ENV` instruction sets the environment variable `<key>` to the value `<value>`. 

```Dockerfile
ENV <key>=<value>
```

for example:

```Dockerfile
ENV NODE_ENV=production
```

## RUN

The `RUN` instruction will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the Dockerfile.

```Dockerfile
RUN <command>
```

for example:

```Dockerfile
RUN npm install
```

or 

```Dockerfile
RUN mkdir /home/app
```

## COPY

The `COPY` instruction copies new files or directories from `<src>` (the host machine) and adds them to the filesystem of the container at the path `<dest>`.

```Dockerfile
COPY <src> <dest>
```

for example:

```Dockerfile
COPY . /app
```

## WORKDIR

The `WORKDIR` instruction sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY` and `ADD` instructions that follow it in the Dockerfile. 

```Dockerfile
WORKDIR <path>
```

for example:

```Dockerfile
WORKDIR /app
```

## CMD

The `CMD` instruction has three forms:

- `CMD ["executable","param1","param2"]` (exec form, this is the preferred form)
- `CMD ["param1","param2"]` (as default parameters to ENTRYPOINT)
- `CMD command param1 param2` (shell form)

There can only be one `CMD` instruction in a Dockerfile. If you list more than one `CMD` then only the last `CMD` will take effect.

The `CMD` instruction is the default command to run when a container is launched. It can be overridden by passing a command to `docker run` (for example, `docker run <image-name> <command>`).

```Dockerfile
CMD <command>
```

for example:

```Dockerfile
CMD npm start
```

## Build the image

To build the image out of the Dockerfile, run the following command:

```bash
$ docker build -t <image-name> .
```

the `-t` flag is used to tag the image with a name, and the `.` tells docker to look for the Dockerfile in the current directory.

# Docker Volumes

Docker volumes allow you to persist data generated by and used by Docker containers. Volumes can be shared and reused between containers.

If I create a file inside a container or made a change in my database then I restart the container, the file or the change will be lost. To persist the data, we need to use volumes.

## Docker Volumes in docker-compose

To create a volume in docker-compose, add the following code to the docker-compose.yml file:

```yml
version: "3.8"
services:
  mongodb:
      image: mongo
      ports:
         - "27017:27017"
      environment:
         - MONGO_INITDB_ROOT_USERNAME=admin
         - MONGO_INITDB_ROOT_PASSWORD=password
      volumes:
         - mongodb-data:/data/db
   mongo-express:
      image: mongo-express
      ports:
         - "8081:8081"
      environment:
         - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
         - ME_CONFIG_MONGODB_ADMINPASSWORD=password
         - ME_CONFIG_MONGODB_SERVER=mongodb
volumes:
   mongodb-data:
      driver: local
```

Let's go through the code line by line:

- `volumes:` tells docker compose that we are going to create a volume.
- `mongodb-data:` is the name of the volume.
- `driver: local` tells docker compose to create a local volume.
- `mongodb:` is the name of the service.
- `volumes:` tells docker compose that we are going to use a volume.
- `mongodb-data:/data/db` tells docker compose to use the volume named mongodb-data and mount it to the /data/db directory inside the container. (this is where mongodb stores its data)





