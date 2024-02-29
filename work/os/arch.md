---
title: Arch linux
description: 
published: true
date: 2024-02-29T06:53:29.855Z
tags: work
editor: markdown
dateCreated: 2024-02-29T04:15:54.071Z
---

# Install Arch linux

https://itsfoss.com/install-arch-linux/

# Troubleshoot

## Disable internal laptop keyboard

```bash
# disable keyboard driver in boot manager
nano /etc/default/grub

# modify the line
# GRUB_CMDLINE_LINUX_DEFAULT="quiet"
# to
# GRUB_CMDLINE_LINUX_DEFAULT="quiet i8042.nokbd"

# Apply setting
sudo update-grub
# or
grub-mkconfig -o /boot/grub/grub.cfg

# reboot
```