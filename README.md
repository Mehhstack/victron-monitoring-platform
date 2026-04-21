# Low-Cost Victron Monitoring Platform with InfluxDB & Grafana

## Overview
This project demonstrates a scalable and cost-effective monitoring solution for distributed solar infrastructure using Victron devices.

## Problem
Victron VRM is powerful but becomes costly and less flexible when scaling across multiple sites. There is also limited customization for unified dashboards and alerting.

## Solution
A Raspberry Pi-based solution was implemented to collect and visualize data:

- Victron GX device running Node-RED
- Node-RED sends telemetry data to InfluxDB
- InfluxDB stores time-series data
- Grafana provides centralized dashboards and alerting

## Architecture
Victron GX → Node-RED → InfluxDB → Grafana

## Features
- Centralized monitoring for multiple sites
- Individual dashboards per site
- Custom alerting (battery, load, faults)
- Low-cost deployment using Raspberry Pi

## Screenshots
(Add your Grafana screenshots here)

## Setup
1. Configure Node-RED on Victron GX
2. Setup InfluxDB database
3. Import Grafana dashboards
4. Configure alerts

## Notes
All configs are sanitized for security.
