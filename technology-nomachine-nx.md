# NoMachine (NX)

## Overview

NoMachine is a proprietary remote desktop solution based on NX technology. It supports good interactive performance and can attach to physical or virtual Linux desktops.

## Install (Debian/Ubuntu)

```bash
wget https://download.nomachine.com/download/<major>/Linux/nomachine_<version>_amd64.deb
sudo dpkg -i nomachine_<version>_amd64.deb
sudo apt -f install
```

## Service Management

```bash
sudo /usr/NX/bin/nxserver --status
sudo /usr/NX/bin/nxserver --restart
```

Default listening port is typically `4000/tcp`.

## GPU and Headless Notes

For headless NVIDIA hosts, a valid Xorg configuration is still needed so a renderable display exists.

Example skeleton:

```conf
Section "Device"
    Identifier "NVIDIA GPU"
    Driver "nvidia"
    BusID "PCI:<BUS_ID_DECIMAL>"
    Option "AllowEmptyInitialConfiguration" "True"
EndSection

Section "Screen"
    Identifier "Screen0"
    Device "NVIDIA GPU"
    DefaultDepth 24
    SubSection "Display"
        Depth 24
        Virtual 1920 1080
    EndSubSection
EndSection
```

## Validation

```bash
ss -ltnp | grep 4000
glxinfo -B
```

## Notes

- Keep NoMachine and NVIDIA driver versions compatible with your distro kernel.
- If remote 3D is unstable, verify local Xorg starts cleanly before troubleshooting NX.
