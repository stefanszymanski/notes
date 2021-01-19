# Control monitor backlight without root permissions

There are two ways for controlling the monitor backlight:

- DDC/CI for external monitors
- Sysfs for builtin laptop monitors

For both `udevd` must be running.

## Common preparations

```sh
# add user to new group
groupadd i2c
usermod -aG i2c stefan
```

## DDC/CI

```sh
# install packages
sudo xbps-install -Sy ddcutil
# load kernel module
m̀kdir -p /etc/modules-load.d
echo i2c-dev > /etc/modules-load.d/i2c.conf
```

Create file `/etc/udev/rules.d/45-i2c.rules` with following content.

```
KERNEL=="i2c-[0-9]*", GROUP="i2c", MODE="0660"
```

## sysfs

Create `/etc/udev/rules.d/45-sysfs-backlight.rules` with following content.

```
ACTION=="add", SUBSYSTEM=="backlight", KERNEL=="acpi_video0", RUN+="/bin/chgrp i2c /sys/class/backlight/%k/brightness"
ACTION=="add", SUBSYSTEM=="backlight", KERNEL=="acpi_video0", RUN+="/bin/chmod g+w /sys/class/backlight/%k/brightness"
```

## Links

- https://www.ddcutil.com/i2c_permissions/
- https://blog.woefe.com/posts/ddc_screen_brightness.html
- https://wiki.archlinux.org/index.php/backlight
