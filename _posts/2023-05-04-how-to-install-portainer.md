---
layout: post
title: How to Install Portainer
date: 2023-05-04
categories: [Docker]
tags: [portainer, docker, containerization]
image:
  path: /assets/img/headers/portainer.png
---


Here you will understand what happens in a container orchestrator, for that we will use a graphical manager called Portainer.

Portainer is a web interface that interacts with docker socket to create new containers and monitor them. Portainer can also be used to view the cluster, manage user authentication and cluster access permissions. Portainer is, in short, an application to manage your containers, whether they are clustered or not. They aim to speed up the handling of these containers at the time of loading and unloading.

## How to install

#### Step 1. Create the volume that Portainer Server will use to store its database

```console
$ docker volume create portainer_data
```

#### Step 2. Download and install the Portainer Server container

```console
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```
> By default, Portainer generates and uses a self-signed SSL certificate to secure port 9443. Alternatively you can provide your own SSL certificate during installation or via the Portainer UI after installation is complete.
{: .prompt-info }

> If you require HTTP port 9000 open for legacy reasons, add the following to your docker run command: ` -p 9000:900 `
{: .prompt-info }

#### Step 3. Portainer Server has now been installed. You can check to see whether the Portainer Server container has started by running docker ps

```console
docker ps
```

Now that the installation is complete, you can log into your Portainer Server instance by opening a web browser and going to:

```console
https://localhost:9443
```

Replace localhost with the relevant IP address or FQDN if needed, and adjust the port if you changed it earlier.
You will be presented with the initial setup page for Portainer Server.