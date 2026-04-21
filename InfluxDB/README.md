# InfluxDB Configuration

## Purpose
InfluxDB is used to store all telemetry data from distributed solar sites. It is purpose-built for time-series data, making it ideal for continuously streaming metrics from Victron GX devices via Node-RED.

It is optimized for:
- High-frequency time-series writes
- Efficient range queries for Grafana dashboards
- Long-term storage of solar, battery, and load metrics

---

## Why InfluxDB

We chose InfluxDB over a traditional relational database because solar telemetry is inherently time-series data — every reading is timestamped and queried by time range. InfluxDB handles this natively with far better performance and storage efficiency than something like PostgreSQL or MySQL would for the same workload.

It also integrates directly with Grafana out of the box, which keeps the stack simple.

---

## Data Structure

### Measurements
| Measurement | Description |
|---|---|
| `battery_voltage` | Battery bank voltage in volts |
| `load_power` | Current load draw in watts |
| `solar_yield` | Solar panel output in watts |
| `system_status` | GX device system state |

### Tags
Tags are indexed metadata used to filter and group queries:

| Tag | Description |
|---|---|
| `site_name` | Human-readable site identifier (e.g. `Tower-01`) |
| `device_id` | Unique Victron GX device ID |

### Fields
Fields are the actual numeric values stored per measurement. Unlike tags, fields are not indexed.

---

## Example Data Point

```json
{
  "measurement": "battery_voltage",
  "tags": {
    "site_name": "Tower-01",
    "device_id": "gx-001"
  },
  "fields": {
    "value": 52.3
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

---

## Retention Policy
Consider setting a retention policy to automatically expire old raw data and keep storage manageable:

```sql
CREATE RETENTION POLICY "90_days" ON "victron" DURATION 90d REPLICATION 1 DEFAULT
```

Adjust duration based on your storage capacity on the Raspberry Pi.

---

## Notes
- All credentials and database names in config files have been sanitized for security.
- Grafana connects to InfluxDB via the local network using the HTTP API.
