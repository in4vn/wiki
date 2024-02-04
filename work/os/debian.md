---
title: Install Debian 12
description: 
published: true
date: 2024-02-04T03:47:48.647Z
tags: work
editor: markdown
dateCreated: 2024-02-04T03:47:48.647Z
---

# Install Debian 12

## Prepare USB Drive

- Download iso file from https://www.debian.org/
- Build iso file to USB Drive using these command
```bash
# find drive list
# look for USB drive (external disk)
# for example /dev/disk4
diskutil list

# unmount disk
diskutil unmountDisk /dev/disk4

# build iso file to external disk
dd if=<path-to-dowloaded-iso-file>  of=/dev/disk4 bs=1m

# wait until complete
```

## Install Debian

- Boot from USB Drive
- Go to Bios settings (press F12 while your laptop booting)
- Select boot from your USB drive
- Follow steps to install

## Fix boot issue

- Reboot and press F2 to enter the bios.
- Go to the `General` section then click `Boot Sequence`
- Make sure the `UEFI` option is checked.
- Click the `Add Boot Option` button, on the `Add Boot Option` panel click the `File Name` button
- Browse to the debian/shimx64.efi file and click Ok
- Enter a name in the `Boot Option Name` field (debian) then click Ok.
- Save the changes and reboot