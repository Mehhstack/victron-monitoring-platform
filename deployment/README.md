# Deployment (Docker on LXC)

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
| LXC OS | Debian 12 |
| Container Runtime | Docker + Docker Compose |
| InfluxDB | v2.0 |
| Grafana | Latest stable |

---

## Proxmox LXC

The LXC is provisioned inside Proxmox VE. Below is the VM as it appears in the Proxmox dashboard:

> 📸 **Screenshot:** Proxmox dashboard showing the monitoring VM (status, CPU, memory allocation)

![Proxmox VM Overview](https://github.com/Mehhstack/victron-monitoring-platform/blob/main/deployment/proxmox-LXC.png)

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

![Docker Containers Running](https://github.com/Mehhstack/victron-monitoring-platform/blob/main/deployment/docker-management.png)

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

---
