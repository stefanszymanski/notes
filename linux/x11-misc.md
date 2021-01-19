# Various X11 notes

## Start X as normal user

Edit `/etc/X11/Xwrapper.config`:

```conf
allowed_users=anybody
needs_root_rights=yes
```

## Configure keymap

Create file `/etc/X11/xorg.conf.d/10-keyboard.conf`:

```
Section "InputClass"
	Identifier "Keyboard"
	MatchIsKeyboard "on"
	Driver "libinput"
	Option "XkbLayout" "de"
EndSection
```

## Nvidia drivers

_
**Note:** I did the following with an old Asus notebook.
Driver version 390 and BusID may be different on other devices.
_

Install packages:

```sh
sudo xbps-install -Su nvidia390 nvidia390-dkms nvidia390-gtklibs nvidia390-libs nvidia390-libs-32bit nvidia390-opencl
```

Create file `/etc/X11/xorg.conf.d/90-nvidia.conf`.

```
Section "Module"
	Load "modesetting"
EndSection

Section "Device"
	Identifier "Nvidia Graphics Card"
	Driver      "nvidia"
	VendorName  "NVIDIA Corporation"
	BoardName   "GeForce Graphics"
	BusID	    "PCI:1:0:0"
	Option      "AllowEmptyInitialConfiguration"
EndSection
```
