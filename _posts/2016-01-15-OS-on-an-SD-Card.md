---
layout: post
title: OS on an SD Card
subtitle: A Personal Guide
tags: [Hardware]
bigimg: /img/path.jpg
---


When flashing an SD card for a single board computer with a pre-built image, for example Raspberry Pi or Odroid, general procedures are listed below:

1. Umount the auto-mounted media

```shell
sudo umount /dev/sdX
```

2. Unpack the downloaded image

```shell
xz -d image.img.xz
```

3. Clear the destination media

```shell
sudo dd if=/dev/zero of=/dev/sdX bs=4M
```

4. Flash the image

```shell
sudo dd if=image.img of=/dev/sdX bs=4M
```

5. Ensure all data is written

```shell
sync
```

6. Check the SD card after flashing

```shell
sudo fdisk -l 
```