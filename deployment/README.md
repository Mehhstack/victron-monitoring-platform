# Deployment (Docker on VM)

## Overview
The monitoring stack is deployed on a virtual machine using Docker containers.

This allows:
- Easy deployment and updates
- Isolation of services
- Scalability for multiple sites

---

## Stack Components

- :contentReference[oaicite:0]{index=0}
- :contentReference[oaicite:1]{index=1}
- Docker (container runtime)

---

## Architecture

Victron GX → Node-RED → InfluxDB (VM) → Grafana (VM)

---

## Environment

- VM hosted on: (add your provider or "local hypervisor")
- OS: Ubuntu Server (or what you used)
- Docker installed and configured

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
