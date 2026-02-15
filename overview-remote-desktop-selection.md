# Architecture and Selection

## Session Models

Remote desktop solutions for Linux usually fit one of these models:

1. **Desktop sharing**: shares an existing local desktop session (`:0`) with remote users.
2. **Virtual desktop/session**: creates a separate remote-only desktop per login.
3. **Relay/P2P remote control**: application-level remote control with optional relay service.

## Technology Comparison

| Technology | Protocol / Stack | Session Model | NAT Traversal | GPU/3D Notes | Best For |
|---|---|---|---|---|---|
| RustDesk | RustDesk protocol (`hbbs` + `hbbr`) | Desktop sharing / remote control | Yes (ID + relay) | App-level remote rendering; not Linux-native GL session forwarding | Cross-network support and unattended access |
| XRDP + xorgxrdp | RDP | Virtual per-user session | No (port forward/VPN needed) | Hardware acceleration depends on DRI3/GLAMOR support and driver stack | Standard Linux remote desktop over RDP |
| NoMachine | NX | Physical or virtual desktop | Limited direct NAT traversal; usually direct reachable host | Good performance, proprietary stack | Turnkey remote desktop |
| TurboVNC + VirtualGL | VNC + OpenGL offload | Usually virtual X session | No (tunnel/VPN recommended) | Strong option for OpenGL workloads | 3D apps, CAD, visualization |
| x11vnc | VNC server for existing X display | Desktop sharing (`:0`) | No (tunnel/VPN recommended) | Uses the existing X server/GPU state | Attach to the active physical/headless display |

## Quick Selection by Use Case

- **Need easiest cross-network remote support**: RustDesk.
- **Need native RDP client compatibility**: XRDP.
- **Need best Linux OpenGL remote workflow**: TurboVNC + VirtualGL.
- **Need to control the active local display**: x11vnc or NoMachine physical desktop mode.

## Security Baseline

- Prefer VPN or SSH tunnels for VNC/X11-based stacks.
- Do not expose management ports directly to the public internet unless required.
- Use strong passwords/keys and enable TLS where supported.
- Keep GPU drivers, Xorg, and remote-desktop services updated.
