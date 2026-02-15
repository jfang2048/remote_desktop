# Headless NVIDIA Linux Guide

## Goal

Prepare a Linux host with NVIDIA GPU for remote desktop without a physical monitor.

## Baseline Checklist

1. Install NVIDIA driver and confirm kernel module is loaded.
2. Ensure a desktop environment is installed (for example XFCE).
3. Create a headless-capable Xorg configuration.
4. Choose a remote stack (`xrdp`, `TurboVNC + VirtualGL`, `NoMachine`, etc.).

## Validate GPU Availability

```bash
nvidia-smi
glxinfo -B
ls -l /dev/dri
```

## Example Headless Xorg Snippet

```conf
Section "Device"
    Identifier "NVIDIA GPU"
    Driver "nvidia"
    BusID "PCI:<BUS_ID_DECIMAL>"
    Option "AllowEmptyInitialConfiguration" "True"
    Option "UseDisplayDevice" "none"
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

Notes:

- Xorg `BusID` uses decimal PCI components (not hex).
- You may need `xserver-xorg-video-dummy` on some setups.
- EDID emulation is optional and only needed on specific hardware/driver combinations.

## Choosing a Remote Path

- **XRDP (`xorgxrdp`)**: better RDP compatibility, variable GPU behavior.
- **TurboVNC + VirtualGL**: usually stronger for OpenGL workloads.
- **x11vnc**: attaches to current display, useful for shared physical/headless session.

## Common Pitfalls

- Remote session starts but rendering is software-only (`llvmpipe`).
- Black screen due to desktop startup script issues.
- Incorrect driver libraries in `LD_LIBRARY_PATH`.
- Access restrictions for `/dev/dri/*` render nodes.
