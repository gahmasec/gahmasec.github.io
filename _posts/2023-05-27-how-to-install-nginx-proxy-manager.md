---
layout: post
title: How to Install Nginx Proxy Manager
date: 2023-05-27
categories: [Docker]
tags: [docker, containerization]
image:
  path: /assets/img/headers/nginxproxymanager.png
---

Nginx Proxy Manager is an open-source web-based application that provides a user-friendly interface for managing reverse proxy configurations with Nginx. It simplifies the process of setting up and managing reverse proxy servers, making it easier to route and proxy incoming web traffic to different applications or services running on separate internal servers.

Here are some key features and functionalities of Nginx Proxy Manager:

1. Web-Based Interface: Nginx Proxy Manager offers a browser-based interface that allows you to configure and manage reverse proxy settings without directly editing Nginx configuration files. This makes it more accessible to users who may not be familiar with the intricacies of Nginx configuration.

2. SSL/TLS Support: The application provides built-in support for generating and managing SSL/TLS certificates for secure HTTPS connections. It simplifies the process of obtaining and renewing SSL certificates, allowing you to secure your proxied services with ease.

3. Domain and Subdomain Management: Nginx Proxy Manager allows you to manage domains and subdomains within the application, making it convenient to set up and manage multiple proxied services under different domains.

4. Proxy Rules and Routing: The application enables you to define proxy rules to route incoming requests to the appropriate backend server or service based on domain, subdomain, path, or other request parameters. This flexibility allows you to direct traffic to different applications or services running on separate internal servers.

5. Authentication and Access Control: Nginx Proxy Manager provides authentication and access control features, allowing you to restrict access to your proxied services using username and password authentication or integration with external authentication providers.

6. Logging and Monitoring: The application offers logging and monitoring capabilities, allowing you to monitor and analyze the traffic flowing through the reverse proxy. It provides insights into request logs, error logs, and other relevant information for troubleshooting and analysis.

Nginx Proxy Manager simplifies the process of managing reverse proxy configurations by providing a user-friendly interface and automating many of the tasks involved in setting up and maintaining Nginx reverse proxy servers. It is a popular choice for individuals and organizations looking to streamline their reverse proxy setup and management processes.

## Installation

- [**Official Documentation**](https://nginxproxymanager.com/guide/#quick-setup)

#### Step 1. Create docker compose file
```console
nano docker-compose.yml
```
#### Step 2. Paste this code

```yaml
version: '3.8'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
```
#### Step 3. Run

```console
$ docker-compose up -d
```
> The port to access the Nginx UI is 81
{: .prompt-info }

## Default Admin User

- **Email: admin@example.com**
- **Password: changeme**