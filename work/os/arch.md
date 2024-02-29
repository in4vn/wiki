---
title: Install Arch Linux
description: 
published: true
date: 2024-02-29T08:34:38.160Z
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

# Keyboard

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

## Mac's style keymap for Linux

Follow the guide on this page
https://github.com/RedBearAK/toshy

If after installed, it's still not working

```bash
nano ~/.config/toshy/toshy_config.py

# change this 
keyboards_UserCustom_dct = {
    # Add your keyboard device here if its type is misidentified.
    # Valid types to map device to: Apple, Windows, IBM, Chromebook (case sensi>
    # Example:
    'My Keyboard Device Name': 'Apple',
}
# to this
keyboards_UserCustom_dct = {
    # Add your keyboard device here if its type is misidentified.
    # Valid types to map device to: Apple, Windows, IBM, Chromebook (case sensi>
    # Example:
    # 'My Keyboard Device Name': 'Apple',
    'BT5.0 KB': 'Apple',
}

# change this
keyboards_Apple = [
    # Add specific Apple/Mac keyboard device names to this list
    'Mitsumi Electric Apple Extended USB Keyboard',
    'Magic Keyboard with Numeric Keypad',
    'Magic Keyboard',
    'MX Keys Mac Keyboard'
]
# to this
keyboards_Apple = [
    # Add specific Apple/Mac keyboard device names to this list
    'Mitsumi Electric Apple Extended USB Keyboard',
    'Magic Keyboard with Numeric Keypad',
    'Magic Keyboard',
    'MX Keys Mac Keyboard',
    'BT5.0 KB'
]

# save and call this command
toshy-services-restart
```