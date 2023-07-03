---
layout: post
title: Share Files With Samba
date: 2023-06-03
categories: [Ubuntu]
tags: [sharing, backup]
image:
  path: /assets/img/headers/samba.png
---

Samba share refers to the sharing of files and resources between different operating systems using the Samba software suite. Samba is an open-source implementation of the Server Message Block (SMB) protocol, which is commonly used for sharing files, printers, and other resources between computers on a network.

Samba allows computers running different operating systems, such as Windows, macOS, Linux, and Unix, to communicate and share files seamlessly. With Samba, a computer can act as a file server, making its directories and files accessible to other computers on the network.

Setting up a Samba share involves configuring a Samba server on the computer that has the files or resources to share. The Samba server manages user authentication and access control, ensuring that only authorized users can access the shared files or directories. Once the Samba server is set up, other computers on the network can connect to the Samba share using their respective operating systems and access the shared files as if they were local to their own system.

Samba shares are widely used in both home and enterprise environments to facilitate file sharing and collaboration between computers with different operating systems. They provide a convenient way to share files and resources across heterogeneous networks.

> SMB (Server Message Block) and CIFS (Common Internet File System) are closely related but not exactly the same. SMB is a protocol that originated from Microsoft and is used for sharing files, printers, and other resources between computers on a network. It provides a way for computers to communicate and access shared resources over a network. CIFS, on the other hand, is an enhanced version of SMB that was developed by Microsoft and other vendors to improve compatibility and extend the functionality of SMB. CIFS introduced additional features and enhancements to the original SMB protocol, including better support for authentication and security, improved file access control, and increased performance. In practice, the terms SMB and CIFS are often used interchangeably, especially in the context of file sharing between Windows systems. Microsoft Windows operating systems typically refer to the file sharing protocol as SMB, while other operating systems, such as Linux, often use the term CIFS. It's worth noting that with the introduction of newer versions of the SMB protocol, such as SMB2 and SMB3, Microsoft has moved away from using the term CIFS and now commonly refers to the protocol as SMB. However, the underlying concepts and functionality remain similar, regardless of whether it's called SMB or CIFS.
{: .prompt-info }

#### Step 1. Install Samba
```console
sudo apt install samba
```

#### Step 2. Open smb.conf file

```console
sudo nano /etc/samba/smb.conf
```

#### Step 3. Paste this...

```console
[sambashare]
comment = Share directory
path = /home/sharing
force user = smbuser
force group = smbgroup
create mask = 0664
force create mode = 0664
directory mask = 0775
force directory mode = 0775
public = yes
read only = no
```

#### Step 4. Create "sharing" folder

```console
cd ~
mkdir -p sharing
```

#### Step 5. Create User and Group

```console
sudo groupadd --system smbgroup
sudo useradd --system --no-create-home --group smbgroup -s /bin/false smbuser
```

#### Step 6. Change permissions

```console
sudo chown -r smbuser:smbgroup ~/sharing
sudo chmod -r g+w ~/sharing
```

#### Step 7. Restart Samba service

```console
sudo systemctl restart smbd
```

After this you are able to access and map this folder in any operating system. 