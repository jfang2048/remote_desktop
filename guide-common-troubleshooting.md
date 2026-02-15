# Troubleshooting

## Fast Diagnostics

```bash
# Services and ports
systemctl status xrdp xrdp-sesman --no-pager
ss -ltnup | grep -E '3389|4000|590[0-9]|2111[5-9]'

# Xorg / GPU
nvidia-smi
glxinfo -B
journalctl -b --no-pager | grep -Ei 'xrdp|xorg|nvidia|glamor|dri3'
```

## Common Issues

| Symptom | Likely Cause | Check / Fix |
|---|---|---|
| XRDP black screen after login | Desktop startup script mismatch | Simplify `/etc/xrdp/startwm.sh`; ensure `~/.xsession` contains a valid desktop command |
| Very slow XRDP redraw | Small TCP send buffer; compositor overhead | Set `tcp_send_buffer_bytes=4194304`; disable desktop compositing if needed |
| OpenGL uses `llvmpipe` | GPU libraries not active in session | Verify driver install, DRI/GLAMOR path, and session environment |
| VNC session starts but no GPU acceleration | VirtualGL not configured or not used | Re-run `vglserver_config`, add user to `vglusers`, launch app with `vglrun` |
| Headless host cannot create display | Missing virtual Xorg configuration | Add `AllowEmptyInitialConfiguration` and virtual screen settings |
| Cannot connect from internet | NAT/firewall/routing issue | Open required ports or use VPN/relay architecture |

## XRDP-Specific Notes

- `--enable-glamor` belongs to `xorgxrdp`, not `xrdp`.
- `xrdp` with Xorg backend behaves differently from `xrdp` over VNC bridge.
- Root remote login is usually disabled by default and should generally stay disabled.

## Validation Commands by Stack

```bash
# XRDP
ss -ltnp | grep 3389

# NoMachine
ss -ltnp | grep 4000

# TurboVNC
ss -ltnp | grep 5901

# RustDesk server
ss -ltnup | grep -E '21115|21116|21117|21118|21119'
```
