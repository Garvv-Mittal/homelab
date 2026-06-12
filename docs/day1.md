# Day 1 — Homelab Setup

**Date:** 11 June 2026

---

## Objective

Convert an old Acer Aspire E1-570 laptop into a dedicated Linux homelab server for learning Linux administration, Docker, networking, self-hosting, and infrastructure management.

---

## Hardware

| Component | Specification          |
|-----------|------------------------|
| Laptop    | Acer Aspire E1-570     |
| CPU       | Intel Core i3-3217U    |
| RAM       | 6 GB                   |
| SSD       | 128 GB                 |
| HDD       | 500 GB                 |
| Network   | Wi-Fi (Atheros AR9565) |

---

## Tasks Completed

### 1. Installed Debian 13 (Headless)

- Created bootable installation media.
- Installed Debian 13 without a desktop environment.
- Selected only:
  - SSH Server
  - Standard System Utilities

**Result:** Lightweight server installation with minimal RAM usage (~365 MB after boot).

---

### 2. Basic System Configuration

Updated system packages and installed essential utilities:

```bash
apt update
apt full-upgrade -y
apt install vim git curl htop btop tmux -y
```

---

### 3. SSH Configuration

Verified SSH service status and established remote connection from a secondary machine, enabling complete headless administration of the server.

```bash
systemctl status ssh
```

---

### 4. Power Management Configuration

Modified `/etc/systemd/logind.conf` to prevent suspension when the laptop lid is closed:

```ini
HandleLidSwitch=ignore
HandleLidSwitchExternalPower=ignore
HandleLidSwitchDocked=ignore
```

**Purpose:** Allow the server to operate continuously with the lid closed.

---

### 5. GRUB Configuration

Modified GRUB timeout settings to reduce boot menu delay, then applied changes:

```bash
update-grub
```

---

### 6. Docker Installation

Installed Docker using the official installation script:

```bash
curl -fsSL https://get.docker.com | sh
```

**Learned:**
- How `curl` fetches and pipes shell scripts
- Docker daemon architecture
- Docker installation process on Debian

---

### 7. Docker Permissions

**Issue encountered:**

```
permission denied while trying to connect to the Docker daemon socket
```

**Resolution:** Added user to the Docker group:

```bash
usermod -aG docker <username>
```

---

### 8. First Docker Container

```bash
docker run hello-world
```

**Learned:**
- Docker Images and Containers
- Docker Hub registry
- Container lifecycle (pull → create → run → exit)

---

### 9. First Self-Hosted Service — Uptime Kuma

Deployed Uptime Kuma via Docker Compose (`compose.yml`).

**Concepts learned:**
- Docker Compose syntax
- Volumes
- Port mapping
- Restart policies

Successfully accessed the Uptime Kuma dashboard over the local network.

---

### 10. Monitoring Setup

Created monitors for:
- Homelab Server
- Uptime Kuma itself

**Purpose:** Track service availability and detect outages.

---

## Issues Encountered

### Docker Permission Error

| | |
|---|---|
| **Cause** | User not part of the Docker group |
| **Resolution** | Added user to Docker group via `usermod` |

---

### Intermittent Connectivity Loss

Investigated potential causes:

- SSH diagnostics
- DHCP lease logs
- Wi-Fi power management settings
- Systemd sleep targets
- Journal logs (`journalctl`)

**Findings so far:**
- Wi-Fi power management disabled ✓
- Suspend targets confirmed inactive ✓
- Potential DHCP lease/network issue — **still under investigation**

---

## Key Concepts Learned

- Headless Linux server setup
- SSH for remote administration
- Systemd service and power management
- Docker — images, containers, lifecycle
- Docker Compose — volumes, port mapping, restart policies
- Basic network troubleshooting

---

## Current Architecture

```
Personal Machine (Arch Linux)
        │
        │ SSH
        ▼
Homelab Server (Debian 13, Headless)
        │
        ▼
      Docker
        │
        ▼
  Uptime Kuma (Port 3001)
```

---

## Next Steps

- [ ] Resolve DHCP/network stability issue
- [ ] Mount and configure 500 GB HDD for storage
- [ ] Deploy Pi-hole
- [ ] Deploy Syncthing
- [ ] Deploy Gitea
- [ ] Learn Docker volumes and networking in depth

---

## Day 1 Outcome

Successfully transformed an old laptop into a functioning headless Debian 13 server running Docker with a self-hosted monitoring service. Foundation is in place for future self-hosted infrastructure and containerised deployments.
