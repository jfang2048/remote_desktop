# VNC + TurboVNC + VirtualGL

## Overview

This stack is commonly used for Linux remote 3D workloads.

- **TurboVNC**: efficient remote desktop transport.
- **VirtualGL**: routes OpenGL rendering to GPU-backed X server and sends frames to VNC session.

## When To Use

- OpenGL-heavy desktop apps (CAD, scientific visualization, browsers with GPU rendering).
- Multi-user remote Linux environments where RDP acceleration is insufficient.

## Install (Debian/Ubuntu)

```bash
sudo apt update
sudo apt install -y mesa-utils xauth xfce4

# Install TurboVNC package
sudo dpkg -i turbovnc_<version>_amd64.deb
sudo apt -f install

# Install VirtualGL package
sudo dpkg -i virtualgl_<version>_amd64.deb
sudo apt -f install
```

## Server Configuration

1. Configure NVIDIA/Xorg for headless or physical display mode.
2. Run VirtualGL server setup:

```bash
sudo /opt/VirtualGL/bin/vglserver_config
```

3. Add users to `vglusers`:

```bash
sudo usermod -aG vglusers <USER>
```

4. Create TurboVNC startup script:

```bash
mkdir -p ~/.vnc
cat > ~/.vnc/xstartup.turbovnc <<'SH'
#!/bin/sh
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
exec /opt/VirtualGL/bin/vglrun /usr/bin/startxfce4
SH
chmod +x ~/.vnc/xstartup.turbovnc
```

## Run and Test

```bash
/opt/TurboVNC/bin/vncserver :1 -vgl

# GPU validation
/opt/VirtualGL/bin/vglrun glxinfo -B
/opt/VirtualGL/bin/vglrun /opt/VirtualGL/bin/glxspheres64
```

## Optional: Attach Existing Display with x11vnc

```bash
x11vnc -display :0 -forever -auth guess
```

Use this only when you need to share an existing local session.

## Troubleshooting

- Browser does not start under `vglrun`: confirm DISPLAY and user session environment.
- Renderer shows `llvmpipe`: validate NVIDIA driver and VirtualGL configuration.
- Headless failures: ensure Xorg starts with `AllowEmptyInitialConfiguration` and virtual screen settings.
