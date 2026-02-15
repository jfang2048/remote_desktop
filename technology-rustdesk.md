# RustDesk

## Overview

RustDesk is a self-hostable remote desktop/control platform with relay and ID services. It is useful when hosts are behind NAT and direct inbound connectivity is limited.

## Components

- `hbbs`: ID/rendezvous service
- `hbbr`: relay service
- RustDesk client on operator and target hosts

Default server ports:

- `hbbs`: `21115/tcp`, `21116/tcp`, `21116/udp`, `21118/tcp`
- `hbbr`: `21117/tcp`, `21119/tcp`

## Server Deployment (Docker Compose)

```yaml
version: "3.8"

services:
  hbbr:
    image: rustdesk/rustdesk-server:latest
    container_name: hbbr
    command: hbbr
    ports:
      - "21117:21117"
      - "21119:21119"
    volumes:
      - ./data:/root
    restart: unless-stopped

  hbbs:
    image: rustdesk/rustdesk-server:latest
    container_name: hbbs
    command: hbbs -r <SERVER_HOST_OR_IP>:21117
    ports:
      - "21115:21115"
      - "21116:21116"
      - "21116:21116/udp"
      - "21118:21118"
    volumes:
      - ./data:/root
    depends_on:
      - hbbr
    restart: unless-stopped
```

```bash
docker compose up -d
```

## Client Install (Debian/Ubuntu)

```bash
# Download a release package from RustDesk GitHub releases.
sudo apt update
sudo apt install ./rustdesk-<version>-x86_64.deb
```

## Headless Linux Basics

```bash
sudo apt install -y xserver-xorg-video-dummy lightdm
sudo rustdesk --get-id
sudo rustdesk --password '<strong-password>'
```

## Validation

- Server containers are running: `docker ps`
- Ports are listening: `ss -ltnup | grep -E '2111[5-9]'`
- Client can register and connect using your self-hosted server settings

## Notes

- Keep server and client versions reasonably aligned.
- If direct connection fails, ensure relay ports and firewall rules are correct.
