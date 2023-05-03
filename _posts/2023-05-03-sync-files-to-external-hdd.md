---
title: Sync files to external HDD
date: 2023-05-03
categories: [Ubuntu]
tags: [synchronization, backup]
---

## Little Explanation
As I have several documents and images that I absolutely do not want to lose, I thought of several ways to achieve redundancy. I use ownCloud and I've already used Resilio Sync, but I wanted to be able to automatically store all the modifications made to my documents on an external HDD (in case I have a disaster on my server). For that I needed to add the two lines below in crontab to get what I wanted.

```console
$ sudo nano /etc/crontab
```

```sh
*/30    * * * * root rsync -avP --delete /home/guilherme/Documents /media/guilherme/New\ Volume/
*/30    * * * * root rsync -avP --delete /home/guilherme/Pictures /media/guilherme/New\ Volume/
```
