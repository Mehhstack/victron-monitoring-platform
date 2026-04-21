# Deployment (Docker on VM)

## Overview
The monitoring stack is deployed on a virtual machine using Docker containers hosted on a Proxmox server.

This allows:
- Easy deployment and updates via docker-compose
- Isolation of services (InfluxDB and Grafana run independently)
- Simple rollback and version pinning
- Scalability across additional sites without re-architecting

---

## Environment

| Component | Detail |
|---|---|
| Hypervisor | Proxmox VE |
| VM OS | Debian 12 |
| Container Runtime | Docker + Docker Compose |
| InfluxDB | v2.0 |
| Grafana | Latest stable |

---

## Proxmox VM

The VM is provisioned inside Proxmox VE. Below is the VM as it appears in the Proxmox dashboard:

> 📸 **Screenshot:** Proxmox dashboard showing the monitoring VM (status, CPU, memory allocation)

![Proxmox VM Overview](screenshots/proxmox-vm-overview.png)

---

## Stack Components

- **InfluxDB** — time-series database for all telemetry storage
- **Grafana** — dashboard and alerting frontend
- **Docker** — container runtime managing both services

---

## Architecture

---

## Docker Container Management

Both services are managed as Docker containers. The screenshot below shows the running containers
and their health status:

> 📸 **Screenshot:** Docker container panel showing influxdb and grafana containers running
> (use `docker ps` output, Portainer, or your container management UI)

![Docker Containers Running](screenshots/docker-containers-running.png)

---

## Docker Setup

### docker-compose.yml

```yaml
version: "3.8"
services:
  influxdb:
    image: influxdb:2.0
    container_name: influxdb
    ports:
      - "8086:8086"
    volumes:
      - influxdb_data:/var/lib/influxdb2
    restart: unless-stopped

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports:
      - "3000:3000"
    volumes:
      - grafana_data:/var/lib/grafana
    restart: unless-stopped

volumes:
  influxdb_data:
  grafana_data:
```

### Starting the Stack

```bash
docker-compose up -d
```

### Checking Container Status

```bash
docker ps
docker-compose logs -f
```

---

## Screenshots

| What to capture | Where to add it |
|---|---|
| Proxmox dashboard with VM visible | `screenshots/proxmox-vm-overview.png` |
| Docker containers running (`docker ps` or Portainer) | `screenshots/docker-containers-running.png` |
| Grafana login or main dashboard | `screenshots/grafana-dashboard.png` |
| InfluxDB data explorer showing incoming data | `screenshots/influxdb-data.png` |

> Create a `screenshots/` folder in this directory and drop your images in there, then the
> image links above will render automatically on GitHub.

---

## Notes
- All credentials and environment variables have been sanitized for security.
- InfluxDB data persists via Docker named volumes — data survives container restarts.
- Grafana runs on port `3000` and InfluxDB on port `8086` — ensure your VM firewall allows
  access from your local network.
