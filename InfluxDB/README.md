# InfluxDB

## Purpose

InfluxDB stores all telemetry data from distributed solar sites. It is purpose-built
for time-series data, making it ideal for continuously streaming metrics from Victron
GX devices via Node-RED.

---

## Why InfluxDB

Solar telemetry is inherently time-series data — every reading is timestamped and
queried by time range. InfluxDB handles this natively with far better performance and
storage efficiency than a relational database would for the same workload. It also
integrates directly with Grafana out of the box, keeping the stack simple.

---

## Data Structure

### Measurements

| Measurement | Description |
|---|---|
| `battery_voltage` | Battery bank voltage in volts |
| `battery_current` | Charge or discharge current in amps |
| `battery_soc` | State of charge as a percentage |
| `load_power` | Current load draw in watts |
| `solar_yield` | Solar panel output in watts |
| `system_status` | GX device system state |

### Tags

| Tag | Description |
|---|---|
| `site_name` | Human-readable site identifier (e.g. `Tower-01`) |
| `device_id` | Unique Victron GX device ID |

### Fields

The actual numeric values stored per measurement. Unlike tags, fields are not indexed
and are not used for filtering — only for the values being graphed or alerted on.

---

## Data Explorer

Incoming measurements as seen in the InfluxDB Data Explorer:

> 📸 Screenshot: InfluxDB Data Explorer showing measurements and incoming data

![InfluxDB Data Explorer](https://github.com/Mehhstack/victron-monitoring-platform/blob/main/InfluxDB/influx-data-explorer.png)

---

## Grafana Integration

InfluxDB is connected to Grafana as a data source over the local Docker network,
using the container name as the hostname (`http://influxdb:8086`).

> 📸 Screenshot: Grafana data source configuration showing InfluxDB connected

![Grafana Data Source](https://github.com/Mehhstack/victron-monitoring-platform/blob/main/InfluxDB/grafana-influxdb-data-source.png)

---

## Retention Policy

A retention policy is set to automatically expire old raw data and keep storage
manageable on the host VM.

| Setting | Value |
|---|---|
| Policy Name | `365_days` |
| Duration | 365 days |
| Applies To | All raw telemetry measurements |

Adjust duration based on available storage on the Proxmox LXC.
