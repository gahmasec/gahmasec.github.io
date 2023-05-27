---
layout: post
title: How to Install Pi-hole
date: 2023-05-27
categories: [Ubuntu]
tags: [dns, ads]
image:
  path: /assets/img/headers/pihole.png
---

Pi-hole is an open-source network-wide ad blocker and DNS sinkhole. It is designed to run on a Raspberry Pi or any other Linux-based system and acts as a DNS server for your network. Its primary purpose is to block advertisements and tracking domains at the network level, providing ad-free browsing and improved privacy for all devices on your network.

Here are some key features and functionalities of Pi-hole:

1. Ad Blocking: Pi-hole uses a combination of blocklists and DNS filtering techniques to block ads, pop-ups, and other unwanted content. It intercepts DNS requests from devices on your network and checks them against its blocklist database. If a domain is flagged as an ad-serving domain, Pi-hole blocks the request, preventing the corresponding ad from being displayed.

2. Network-Wide Protection: Once installed and configured, Pi-hole provides ad-blocking protection for all devices on your network, including computers, smartphones, tablets, smart TVs, and IoT devices. By blocking at the DNS level, it doesn't require individual ad-blockers or modifications on each device.

3. Improved Performance: By blocking ads and unwanted content, Pi-hole reduces the amount of data being transferred over your network, leading to faster browsing speeds and reduced bandwidth consumption. It can also improve the overall performance of your network by reducing the load on devices and improving page load times.

4. Privacy Enhancement: Pi-hole blocks tracking domains and prevents your devices from connecting to known trackers. This helps protect your privacy by limiting the amount of data collected by advertisers and reducing the potential for targeted advertising.

5. Customization and Whitelisting: Pi-hole allows you to customize its blocklists and whitelists, giving you control over what content is blocked or allowed. You can add specific domains to the whitelist to ensure that certain websites or services are not affected by the ad-blocking.

6. Statistics and Reporting: Pi-hole provides a web interface where you can view statistics and reports on the blocked domains, query types, and overall network activity. This allows you to monitor the effectiveness of Pi-hole and gain insights into the ad traffic on your network.

Pi-hole is a powerful ad-blocking solution that provides network-wide protection against ads and unwanted content. It is particularly popular for home networks and small businesses, offering an easy-to-use and effective way to enhance browsing experience and privacy across multiple devices.

## Installation

- [**Official Documentation**](https://docs.pi-hole.net/main/basic-install/)

#### One-Step Automated Install

```console
curl -sSL https://install.pi-hole.net | bash
```
#### Alternative: Use Docker to deploy Pi-hole

```yaml
version: "3"

# More info at https://github.com/pi-hole/docker-pi-hole/ and https://docs.pi-hole.net/
services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    # For DHCP it is recommended to remove these ports and instead add: network_mode: "host"
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp" # Only required if you are using Pi-hole as your DHCP server
      - "80:80/tcp"
    environment:
      TZ: 'America/Chicago'
      # WEBPASSWORD: 'set a secure password here or it will be random'
    # Volumes store your data between container upgrades
    volumes:
      - './etc-pihole:/etc/pihole'
      - './etc-dnsmasq.d:/etc/dnsmasq.d'
    #   https://github.com/pi-hole/docker-pi-hole#note-on-capabilities
    cap_add:
      - NET_ADMIN # Required if you are using Pi-hole as your DHCP server, else not needed
    restart: unless-stopped
```
#### If you need to change the Web UI port:

```console
nano /etc/lighttpd/lighttpd.conf
```
```code
 server.modules = (
        "mod_indexfile",
        "mod_access",
        "mod_alias",
        "mod_redirect",
)

server.document-root        = "/var/www/html"
server.upload-dirs          = ( "/var/cache/lighttpd/uploads" )
server.errorlog             = "/var/log/lighttpd/error.log"
server.pid-file             = "/run/lighttpd.pid"
server.username             = "www-data"
server.groupname            = "www-data"
server.port                 = 8080 #CHANGE THIS

# features
#https://redmine.lighttpd.net/projects/lighttpd/wiki/Server_feature-flagsDetails
server.feature-flags       += ("server.h2proto" => "enable")
server.feature-flags       += ("server.h2c"     => "enable")
server.feature-flags       += ("server.graceful-shutdown-timeout" => 5)
#server.feature-flags       += ("server.graceful-restart-bg" => "enable")

# strict parsing and normalization of URL for consistency and security
# https://redmine.lighttpd.net/projects/lighttpd/wiki/Server_http-parseoptsDetai                                                                                                                              ls
# (might need to explicitly set "url-path-2f-decode" = "disable"
#  if a specific application is encoding URLs inside url-path)
server.http-parseopts = (
  "header-strict"           => "enable",# default
  "host-strict"             => "enable",# default
  "host-normalize"          => "enable",# default
  "url-normalize-unreserved"=> "enable",# recommended highly
  "url-normalize-required"  => "enable",# recommended
  "url-ctrls-reject"        => "enable",# recommended
  "url-path-2f-decode"      => "enable",# recommended highly (unless breaks app)
 #"url-path-2f-reject"      => "enable",
  "url-path-dotseg-remove"  => "enable",# recommended highly (unless breaks app)
 #"url-path-dotseg-reject"  => "enable",
 #"url-query-20-plus"       => "enable",# consistency in query string
)

index-file.names            = ( "index.php", "index.html" )
url.access-deny             = ( "~", ".inc" )
static-file.exclude-extensions = ( ".php", ".pl", ".fcgi" )

# default listening port for IPv6 falls back to the IPv4 port
include_shell "/usr/share/lighttpd/use-ipv6.pl " + server.port
include_shell "/usr/share/lighttpd/create-mime.conf.pl"
include "/etc/lighttpd/conf-enabled/*.conf"

#server.compat-module-load   = "disable"
server.modules += (
        "mod_dirlisting",
        "mod_staticfile",
)
```
#### If you need to use the pi-hole API:

```console
nano /etc/pihole/setupVars.conf
```
```code
PIHOLE_INTERFACE=eth0
QUERY_LOGGING=true
INSTALL_WEB_SERVER=true
INSTALL_WEB_INTERFACE=true
LIGHTTPD_ENABLED=true
CACHE_SIZE=10000
DNS_FQDN_REQUIRED=true
DNS_BOGUS_PRIV=true
DNSMASQ_LISTENING=local
WEBPASSWORD=YOUR_API_CODE
BLOCKING_ENABLED=true
WEBUIBOXEDLAYOUT=boxed
WEBTHEME=default-darker
PIHOLE_DNS_1=1.1.1.1
PIHOLE_DNS_2=1.0.0.1
PIHOLE_DNS_3=2606:4700:4700::1111
DNSSEC=false
REV_SERVER=false
```
