# InfluxDB Configuration

## Purpose
:contentReference[oaicite:3]{index=3} is used to store all telemetry data from distributed solar sites.

It is optimized for:
- Time-series data
- High write performance
- Efficient querying for dashboards

---

## Data Structure

### Measurements
- battery_voltage
- load_power
- solar_yield
- system_status

### Tags
- site_name
- device_id

---

## Example Data

```json
{
  "measurement": "battery_voltage",
  "tags": {
    "site": "Tower-01"
  },
  "fields": {
    "value": 52.3
  }
}
