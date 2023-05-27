---
layout: post
title: How to Install Heimdall
date: 2023-05-27
categories: [Docker]
tags: [docker, containerization]
image:
  path: /assets/img/headers/heimdall.png
---


Heimdall is an open-source project that provides a self-hosted dashboard for managing and monitoring various services and applications within a homelab or small-scale infrastructure. It aims to offer a unified and customizable interface to access and control different services, such as media servers, home automation systems, network monitoring tools, and more.

With Heimdall, you can create a personalized dashboard that aggregates information and links to your preferred services, eliminating the need to access each application individually. It provides a visual and intuitive way to organize and access your services, enhancing the overall management experience.

Heimdall supports a wide range of applications and services through its plugin system, allowing you to integrate popular tools like Plex, Sonarr, Radarr, Transmission, and many others into a single interface. This simplifies the process of monitoring and interacting with your various applications.

The Heimdall dashboard offers features like customizable layouts, dark mode support, search functionality, and the ability to add your own custom links and icons. It also provides a central point for managing user authentication and permissions, ensuring secure access to your services.

## Installation

- [**Official Documentation**](https://hub.docker.com/r/linuxserver/heimdall/)

#### Step 1. Create docker compose file

```console
$ nano docker-compose.yml
```
#### Step 2. Paste this code

```yaml
---
version: "2.1"
services:
  heimdall:
    image: lscr.io/linuxserver/heimdall:latest
    container_name: heimdall
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
    volumes:
      - /path/to/appdata/config:/config
    ports:
      - 80:80
      - 443:443
    restart: unless-stopped
```

#### Step 3. Run

```console
$ docker-compose up -d
```
That's it! You should now have Heimdall up and running using Docker. Remember to customize and configure Heimdall according to your needs by accessing the dashboard and adding the desired services and applications to your dashboard layout.