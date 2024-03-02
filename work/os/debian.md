---
title: Install Debian
description: 
published: true
date: 2024-03-02T06:34:14.474Z
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
dd if=<path-to-dowloaded-iso-file> of=/dev/disk4 bs=1m

# wait until complete
```

## Install Debian

- Boot from USB Drive
- Go to Bios settings (press F12 while your laptop booting)
- Select boot from your USB drive
	- Note: If your laptop won't boot from usb, try to disable `Secure Boot`
- Follow steps to install

## Install Fcitx Unikey

- Go to Software store
- Install Fcitx 5
- Install Unikey addon
- Go to terminal
- Config to use Fcitx 
```bash
im-config
```
- Config Fcitx
```bash
fcitx5-configtool
```
- Add Unikey as Input method
- Go to Global Options, uncheck Show preedit in application, this will disable the text underline
- Enable Fcitx in Firefox
```bash
nano ~/.bash_profile

# add these lines and reload the shell
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
```
- Reboot

## Troubleshoot
### Fix boot issue

- Reboot and press F2 to enter the bios.
- Go to the `General` section then click `Boot Sequence`
- Make sure the `UEFI` option is checked.
- Click the `Add Boot Option` button, on the `Add Boot Option` panel click the `File Name` button
- Browse to the `debian` then `shimx64.efi` file and click Ok
- Enter a name in the `Boot Option Name` field (debian) then click Ok.
- Save the changes and reboot

### Disable internal laptop keyboard

```bash
# login to root
su

# disable keyboard driver in boot manager
nano /etc/default/grub

# modify the line 
# GRUB_CMDLINE_LINUX_DEFAULT="quiet"
# to 
# GRUB_CMDLINE_LINUX_DEFAULT="quiet i8042.nokbd"

# Apply setting
sudo update-grub

# reboot
```

### Mac's style keymap for Linux

Follow the guide on this page
https://github.com/RedBearAK/toshy