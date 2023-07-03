---
layout: post
title: Icons remain after uninstalling program [Wine]
date: 2023-06-03
categories: [Ubuntu]
tags: [wine, maintenance]
image:
  path: /assets/img/headers/wine.png
---

Wine (which stands for "Wine Is Not an Emulator") is an open-source software that implements the Windows API (Application Programming Interface) on top of the POSIX-compliant operating systems like Linux. It provides a translation layer that allows Windows applications to run directly on Linux without the need for a full Windows operating system.

By using Wine, Ubuntu users can run many Windows applications on their Linux system. Wine provides support for a wide range of Windows software, including productivity tools, games, utilities, and more. It aims to provide compatibility and functionality similar to what users would experience running those applications on a Windows system.

To use Wine on Ubuntu, you can install it from the official Ubuntu repositories or use the Wine project's own repositories. Once installed, you can run Windows applications by either double-clicking on the executable file or using the Wine command-line interface. Wine provides a graphical interface to manage and configure the installed applications, as well as compatibility settings.

It's important to note that while Wine is quite capable and supports many Windows applications, not all software will work flawlessly. Compatibility can vary depending on the specific application and its dependencies. The Wine project maintains a database known as the Wine Application Database (AppDB) where users can check the compatibility of specific applications and find tips and workarounds for better performance.

Sometimes when we remove some program that we installed through wine, the icons may still remain on the dashboard. The solution is quite simple.

#### Step 1. Folder location
In this folder you will find all the icons, choose the one you want and remove.
```console
cd ~/.local/share/applications/wine/programs
```
After this you only need to restart your machine