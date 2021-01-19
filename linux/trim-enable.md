# Enable TRIM

## LUKS

Edit `/etc/crypttab` and add option `discard`, e.g.: `crypt /dev/nvme0n1p2 /boot/volume.key luks,discard`.

## LVM

Ensure option `issue_discards = 1` is set in section `devices { ... }` in `/etc/lvm/lvm.conf`.

## Grub

If it's an encrypted boot partition, add `rd.luks.allow-discards` to `GRUB_CMDLINE_LINUX_DEFAULT` in `/etc/default/grub`.

## Cron

Don't enable realtime discards, but run `fstrim` once a day.
Add file `/etc/cron.daily/fstrim` with following content:

```sh
#!/bin/sh

fstrim /
```
