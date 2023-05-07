---
layout: post
title: How to Install Passbolt (Docker)
date: 2023-04-25 
categories: [Docker]
tags: [docker, containerization, security]
image:
  path: /assets/img/headers/passbolt.png
---


Passbolt is a credential manager designed for corporate and/or personal use. It has Community Edition (CE) versions, which is free and open source, and paid versions, including self-hosted (Cloud Version). 

I decided to install passbolt in my homelab using docker, to take all the advantages that containerization has to offer:

- **Significant resource savings**

- **Increased system availability**

- **Easy management**

Some disadvantages in the Passbolt service that I have to point out:
- **Basic users can access a lot of unnecessary information**
- **Browser extension is required**
- **Whenever you login on any new device, you have to use the passbolt-recovery-kit file and create a new password**

## Installation

#### Step 1. Create a folder for Passbolt

```console
$ sudo mkdir /usr/local/bin/dockers/passbolt
```

#### Step 2. Download "docker-compose-ce.yaml" file

```console
$ wget "https://download.passbolt.com/ce/docker/docker-compose-ce.yaml"
```
#### Step 3. Download "docker-compose-ce-SHA512SUM.txt" file

```console
$ wget "https://github.com/passbolt/passbolt_docker/releases/latest/download/docker-compose-ce-SHA512SUM.txt"
```
#### Step 5. Run this command (will verify that the file was downloaded correctly and that there was no problem along the way)

```console
$ sha512sum -c docker-compose-ce-SHA512SUM.txt && echo "Checksum OK" || (echo "Bad checksum. Aborting" && rm -f docker-compose-ce.yaml)
```

#### Step 5. Edit the file "docker-compose-ce.yaml"

```yaml
version: '3.9'
services:
  db:
    image: mariadb:10.10
    restart: unless-stopped
    environment:
      - MYSQL_RANDOM_ROOT_PASSWORD=true
      - MYSQL_DATABASE=passbolt
      - MYSQL_USER=example #choose one
      - MYSQL_PASSWORD=example #choose one
    volumes:
      - database_volume:/var/lib/mysql

  passbolt:
    image: passbolt/passbolt:latest-ce
    #Alternatively you can use rootless:
    #image: passbolt/passbolt:latest-ce-non-root
    restart: unless-stopped
    depends_on:
      - db
    environment:
      - APP_FULL_BASE_URL=https://passbolt.example.xyz #Paste your domain
      - DATASOURCES_DEFAULT_HOST=db
      - DATASOURCES_DEFAULT_USERNAME=example #choose one
      - DATASOURCES_DEFAULT_PASSWORD=example #choose one
      - DATASOURCES_DEFAULT_DATABASE=passbolt
      - EMAIL_TRANSPORT_DEFAULT_HOST=smtp.gmail.com #If you use Gmail, do not change any of these parameters
      - EMAIL_TRANSPORT_DEFAULT_PORT=587
      - EMAIL_TRANSPORT_DEFAULT_USERNAME=$EMAIL_TRANSPORT_DEF>
      - EMAIL_TRANSPORT_DEFAULT_PASSWORD=$EMAIL_TRANSPORT_DEF>
      - EMAIL_TRANSPORT_DEFAULT_TLS=true
      - EMAIL_DEFAULT_FROM=example@gmail.com #Paste your gmail address
    volumes:
      - gpg_volume:/etc/passbolt/gpg
      - jwt_volume:/etc/passbolt/jwt
    command: ["/usr/bin/wait-for.sh", "-t", "0", "db:3306", ">
    ports:
      - 8082:80
      - 4433:443
    #Alternatively for non-root images:
    # - 80:8080
    # - 443:4433

volumes:
  database_volume:
  gpg_volume:
  jwt_volume:
```
#### Step 6. Create your container

```console
$ docker-compose -f docker-compose-ce.yaml up -d
```
> If all goes well you will see a Passbolt container and another Passbolt-db (MariaDB) installed.
{: .prompt-info }

#### Step 7. Create your container
Open your address https://passbolt.example.xyz and follow all the steps that will be asked

#### Step 8. Configure Email
In the email settings, it is normal when you enter your e-mail and password an error appears. This is because you cannot use your password, you need to create a password in "App passwords" in your Google account. And it will be this password that you will use for Passbolt to establish communication with your Gmail account and allow send emails.

> It is very important that you have your e-mail configured, otherwise you will not receive any e-mails. And of course, don't lose your passbolt-recovery-kit.
{: .prompt-info }

This way you can have a self-hosted password manager. Now it's up to you to know if it's worth it