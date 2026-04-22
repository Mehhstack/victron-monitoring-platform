# Node-RED

## Purpose

Node-RED runs directly on the Raspberry Pi (VenusOS) and reads live data from the
devices connected to it — the battery monitor and solar charger. It formats that data
and writes it to InfluxDB where Grafana picks it up for dashboards and alerting.

Without Node-RED, there is no data pipeline — it is the engine of the whole platform.

---

---

## What It Collects

### Battery Monitor
| Metric | Description |
|---|---|
| Battery voltage | Voltage of the battery bank in volts |
| Battery current | Charge or discharge current in amps |
| State of charge | Battery percentage remaining |

### Solar Charger
| Metric | Description |
|---|---|
| PV power | Live solar output in watts |
| Daily yield | Total solar energy produced today in kWh |
| Charger state | Bulk / Absorption / Float / Off |

---

## Flows

### Battery Flow

Reads battery monitor data directly from the Pi and writes voltage, current,
and SOC to InfluxDB on each update.

![Node-RED Battery Flow](node-red%20battery%20flow.png)

### Solar Charger Flow

Reads solar charger data from the Pi and writes PV power output and daily
yield to InfluxDB.

![Node-RED Solar Charger Flow](node-red%20solar%20charger%20flow.png)

---

## Key Logic

- **Victron input nodes** — reads live data directly from connected devices on the Pi
- **Filter** — drops null or invalid readings before they reach the database
- **Transform** — formats raw values into tagged JSON with `site_name` and `device_id`
- **InfluxDB out** — writes the structured payload to the correct measurement

### Example Payload

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

## Setup

1. Install Node-RED via VenusOS Large image
   `Settings → Venus OS Large → Install`
2. Open Node-RED at `http://<pi-ip>:1881`
3. Import the flow files from this folder
4. Update the `site_name` and `device_id` variables to match your site
5. Update the InfluxDB node with your database IP and credentials
6. Deploy and verify data is arriving in InfluxDB
