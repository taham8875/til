# Docker

![Docker](Docker-logo.png)


# What is Docker?

Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and deploy it as one package.

# Why Docker?

Docker solves the problem of "It is running on my machine". Docker allows you to run your application in the same environment everywhere. It is a tool that allows you to deploy your application in a container. If the application works on your machine, it will work on any machine (containing the production server)

Docker also help the developers work on different projects with different dependencies on the same machine without any conflicts  or the hassle of installing and uninstalling dependencies.

# Containers vs Virtual Machines

Containers and virtual machines have similar resource isolation and allocation benefits, but function differently because containers virtualize the operating system instead of hardware. Containers are more portable and efficient.

In short, virtual machines have their own OS and containers share the OS of the host machine.

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




