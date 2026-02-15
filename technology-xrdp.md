# XRDP

## Overview

XRDP provides Microsoft RDP-compatible remote desktop access for Linux.

Two common backends:

1. **Xorg backend (`xorgxrdp`)**: creates per-user remote sessions.
2. **VNC backend (`libvnc.so`)**: bridges XRDP to a VNC server such as `x11vnc`.

## When To Use

- Use XRDP when users need standard RDP client compatibility.
- Use `xorgxrdp` for cleaner per-user Linux sessions.
- Use VNC bridge only when you must attach to an existing display.

## Install (Package-Based)

```bash
sudo apt update
sudo apt install -y xrdp xorgxrdp xfce4
```

Set a desktop session for each user:

```bash
echo xfce4-session > ~/.xsession
```

## Build From Source (Optional)

If you need experimental GLAMOR support, build **xorgxrdp** with `--enable-glamor`.

Important: `--enable-glamor` is an `xorgxrdp` option, not an `xrdp` option.

```bash
# xorgxrdp
./bootstrap
./configure --enable-glamor
make
sudo make install

# xrdp
./bootstrap
./configure
make
sudo make install
```

## Minimal Configuration

### `/etc/xrdp/xrdp.ini`

```ini
[Globals]
port=3389
security_layer=tls
crypt_level=high
bitmap_compression=true

[Xorg]
name=Xorg
lib=libxup.so
username=ask
password=ask
ip=127.0.0.1
port=-1
code=20
```

### `/etc/xrdp/startwm.sh`

Keep this script simple and avoid desktop-environment conflicts.

```sh
#!/bin/sh
unset DBUS_SESSION_BUS_ADDRESS
unset XDG_RUNTIME_DIR
exec startxfce4
```

### Optional Performance Tuning

For high-resolution slow redraw cases:

```ini
# /etc/xrdp/xrdp.ini
tcp_send_buffer_bytes=4194304
```

Then restart:

```bash
sudo systemctl restart xrdp
```

## GPU Notes

- GPU acceleration in XRDP sessions is environment-dependent.
- With Xorg backend, DRI3/GLAMOR support in your stack can improve rendering.
- If GPU acceleration is critical for OpenGL-heavy apps, TurboVNC + VirtualGL is often more reliable.

## Headless and NVIDIA Notes

- For headless hosts, configure an Xorg virtual display (see `guide-headless-nvidia-setup.md`).
- Ensure Xorg can start for non-console XRDP sessions when appropriate:

```conf
# /etc/X11/Xwrapper.config
allowed_users=anybody
needs_root_rights=yes
```

Use this only when required and with controlled access.

## Validation

```bash
systemctl status xrdp xrdp-sesman --no-pager
ss -ltnp | grep 3389
glxinfo -B
```

## Troubleshooting Pointers

- Black screen: verify `startwm.sh`, desktop package, and user session files.
- Software renderer only: verify driver path and DRI3/GLAMOR readiness.
- Session starts but no desktop: check `~/.xsession` and `/var/log/xrdp*`.
