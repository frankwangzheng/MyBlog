---
layout: post
title: Change Default GPU for Display on Ubuntu Startup
subtitle: A Simple Guide
tags: [Hardware]
bigimg: /img/path.jpg
---

When there are multiple NVIDIA GPUs in the computer, I always prefer to setup the display GPU to be the less powerful one, leaving the better one entirely for computation. And it is quite straightforward.

#### 1. Find the Bus ID
First we shall find the Bus ID information for each GPU. We can find it in NVIDIA X-Server settings, by typing the following command in terminal:

```shell
sudo nvidia-settings
``` 

Assuming the bus ID for display-dedicated GPU is "PCI:4:0:0"


#### 2. Create and Edit `xorg.conf` file

Then create a new xorg config file(/etc/X11/xorg.conf) by typing the following command:

```shell
sudo nvidia-xconfig
```

Then edit the Device&Screen section of the xorg config file to looks like this:

```shell
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BusID          "PCI:4:0:0"
EndSection

Section "Screen"
    Identifier     "Screen0"
    Device         "Device0"
    Monitor        "Monitor0"
    DefaultDepth    24
    SubSection     "Display"
        Depth       24
    EndSubSection
EndSection
```

#### 3.Reboot
Ensure the monitor is connected to the display-dedicated GPU. Then reboot for the new xorg config to take effect. We can monitor the GPU usage by typing the following command in terminal:

```shell
watch -n 0.5 nvidia-smi
```
