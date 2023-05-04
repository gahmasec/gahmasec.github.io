---
layout: post
title: How to Full Install Docker Ubuntu
date: 2023-04-25 
categories: [Docker]
tags: [docker, containerization]
image:
  path: /assets/img/headers/docker.png
---


In short, we can conclude that Docker is an open platform, developed with the aim of facilitating the development, deployment and execution of applications in isolated environments. It was specifically designed to deliver an application as quickly as possible. Using Docker, we can easily manage the application infrastructure, thus streamlining the process of construction, maintaining and modifying your service. Docker enabled a common "language" between developers and server administrators. This new "language" is used to build files with the definitions of the necessary infrastructure and how the application will be placed in that environment, on which port it will provide its service, which data from external volumes will be requested and other possible needs. Docker also provides a public cloud for sharing ready-made environments, which can be used to enable customizations for specific environments. Docker uses the container model to "package" the application which, after being transformed into a Docker image, can be reproduced on any platform; that is, if the application works flawlessly on your desktop, it will also work on the server or mainframe. Build once, run anywhere. Containers are isolated at the disk, memory, processing and network levels. This separation allows for great flexibility, where different environments can coexist on the same host, without causing any problems. It is worth noting that the overhead in this process is the minimum necessary, as each container normally carries only one process, which is responsible for delivering the desired service.

## Prerequisites

- [**A 64-bit installation**]
- [**Version 3.10 or higher of the Linux kernel**]

Other prerequisites can be found at [Docker Docs](https://docs.docker.com/engine/install/) in case you need further clarification.

## Installation

### Docker

All steps for a perfect installation.

- [**Official Documentation**](https://docs.docker.com/engine/install/ubuntu/) - All the steps described here are exactly the same as in the official documentation.

#### Step 1. Update the apt package index

```console
$ sudo apt-get update
```
#### Step 2. To install the latest version, run

```console
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

#### Step 3. Verify that the Docker Engine installation is successful by running the hello-world image

```console
$ sudo docker run hello-world
```

> If you want to install a specific version, you must list the versions available in the repository `$ apt-cache madison docker-ce | awk '{ print $3 }'` after the listing select the desired version and install.
{: .prompt-info }

### Manage Docker as a non-root user

#### Step 1. Create the docker group.

```console
$ sudo groupadd docker
```

#### Step 2. Add your user to the docker group.

```console
$ sudo usermod -aG docker $USER
```

#### Step 3. Log out and log back in so that your group membership is re-evaluated.

### Docker-Compose

#### Step 1. To download and install Compose standalone, run

```console
$ sudo curl -SL https://github.com/docker/compose/releases/download/v2.17.2/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
```

#### Step 2. Apply executable permissions to the standalone binary in the target path for the installation.

```console
$ sudo chmod +x /usr/local/bin/docker-compose
```

#### Step 3. Test and execute compose commands using docker-compose

```console
$ docker-compose --version
```
