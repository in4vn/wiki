---
title: Install Arch Linux
description: 
published: true
date: 2024-02-29T07:27:47.553Z
tags: work
editor: markdown
dateCreated: 2024-02-29T04:15:54.071Z
---

# Install Arch Linux 2024.02.01

https://itsfoss.com/install-arch-linux/

# Update

```bash
sudo pacman -Syu
```

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