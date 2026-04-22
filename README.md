# Low-Cost Victron Monitoring Platform with InfluxDB & Grafana

## Overview
This project demonstrates a scalable and cost-effective monitoring solution for distributed solar infrastructure using Victron devices.

## Why I Built This

### Cost-Effective at Scale
Victron VRM is a solid cloud platform, but the costs grow fast when you're managing multiple sites. By running everything on **Raspberry Pis** — affordable, low-power single-board computers — we dramatically cut infrastructure costs without sacrificing visibility. A Raspberry Pi can run Node-RED, InfluxDB, and Grafana simultaneously for a fraction of what cloud-hosted alternatives cost per site.

### Fully Local — No Internet Dependency
This is the big one. If the **Victron VRM site or API goes down**, most cloud-dependent monitoring setups go dark with it. Our platform is **locally hosted**, meaning:

- All data is stored on-site in InfluxDB
- Grafana dashboards remain fully accessible on the local network
- Alerting continues to function regardless of internet connectivity
- You are never dependent on a third-party uptime to monitor your own infrastructure on the Raspberry Pi

This is critical for solar sites in remote areas or where internet reliability is inconsistent.

### Full Customisation
VRM's dashboards are good but opinionated. Running our own Grafana instance gives us complete control over layouts, metrics, thresholds, and alerting logic — tailored exactly to how each site needs to be monitored.

---

## Problem
Victron VRM is powerful but becomes costly and less flexible when scaling across multiple sites. There is also limited customization for unified dashboards and alerting.

## Solution
A Raspberry Pi-based solution was implemented to collect and visualize data:
- Victron GX device running Node-RED
- Node-RED sends telemetry data to InfluxDB
- InfluxDB stores time-series data
- Grafana provides centralized dashboards and alerting

## Architecture

## Features
- Centralized monitoring for multiple sites
- Individual dashboards per site
- Custom alerting (battery, load, faults)
- Low-cost deployment using Raspberry Pi
- **Fully local — works when VRM/internet is down**

## Screenshots
(Add your Grafana screenshots here)

## Setup
1. Configure Node-RED on Victron GX
2. Setup InfluxDB database
3. Import Grafana dashboards
4. Configure alerts
