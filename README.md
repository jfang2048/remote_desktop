# Remote Desktop Documentation (Linux-Focused)

This repository contains practical documentation for Linux remote desktop setups, with emphasis on self-hosted deployment, headless systems, and optional GPU acceleration.

## What Is Included

- Technology guides for:
  - RustDesk
  - XRDP
  - NoMachine (NX)
  - VNC with TurboVNC + VirtualGL
- Architecture and selection guidance
- Headless NVIDIA setup notes
- Common troubleshooting steps

## Documentation Structure

```text
.
|-- README.md
|-- overview-remote-desktop-selection.md
|-- technology-rustdesk.md
|-- technology-xrdp.md
|-- technology-nomachine-nx.md
|-- technology-turbovnc-virtualgl.md
|-- guide-headless-nvidia-setup.md
`-- guide-common-troubleshooting.md
```

## Start Here

1. Read `overview-remote-desktop-selection.md` to choose a stack.
2. Open one of the `technology-*.md` guides for implementation details.
3. If your host has no monitor and uses NVIDIA, read `guide-headless-nvidia-setup.md`.
4. Use `guide-common-troubleshooting.md` for known issues and validation steps.

## Scope

- Primary target: Debian/Ubuntu-like Linux hosts
- Focus: practical setup and operations
- Public-safe examples: no personal hostnames, usernames, or private environment details
