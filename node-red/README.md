# Node-RED

## Purpose

Node-RED runs directly on the Raspberry Pi (VenusOS) and reads live data from the
devices connected to it — the Pylontech battery and solar charger. It formats that
data and writes it to InfluxDB where Grafana picks it up for dashboards and alerting.

Without Node-RED, there is no data pipeline — it is the engine of the whole platform.

---

## Data Flow

```
Pylontech Battery / Solar Charger (Victron input nodes)
        │
        ▼
   Join Node (waits for all readings)
        │
        ▼
  Function Node (structures payload)
        │
        ▼
   InfluxDB Server
```

---

## What It Collects

### Battery Stats

| Metric | Unit |
|---|---|
| Battery power | W |
| Battery current | A |
| Battery temperature | °C |
| Battery voltage | V |
| State of charge | % |
| Capacity | Ah |

### Solar Charger Stats

| Metric | Unit |
|---|---|
| PV power output | W |
| Daily solar yield | kWh |
| Charger state | Bulk / Absorption / Float / Off |

---

## Flows

### Battery Flow

![Battery Flow](node-red%20battery%20flow.png)

Six Victron input nodes read directly from the Pylontech battery over the local
device connection on the Pi — one node per metric. All six feed into a **join node**
configured in manual mode, which waits until it has received a message from all six
inputs before passing anything downstream.

This is intentional — it ensures every write to InfluxDB is a complete record
containing all six metrics together, rather than six separate partial writes arriving
at different timestamps. Without this, cross-metric queries in Grafana become
unreliable as readings for the same moment are scattered across multiple data points.

![Battery flow join](https://github.com/Mehhstack/victron-monitoring-platform/blob/main/node-red/battery-flow-join.png)

The **function node** then takes the combined object, parses each value with
`parseFloat` to ensure clean numeric types, structures them into a single payload,
and sets `msg.measurement = "battery"` before writing to InfluxDB.

![Battery flow function](https://github.com/Mehhstack/victron-monitoring-platform/blob/main/node-red/battery-flow-function.png)

---

### Solar Charger Flow

The solar charger flow follows the same pattern — Victron input nodes feed into a
join node, which waits for a complete set of readings before the function node
structures and forwards the payload to InfluxDB.

![Solar Charger Flow](node-red%20solar%20charger%20flow.png)

![Solar charger join](https://github.com/Mehhstack/victron-monitoring-platform/blob/main/node-red/solar-charger-join.png)

![Solar charger function](https://github.com/Mehhstack/victron-monitoring-platform/blob/main/node-red/solar-charger-function.png)
---

## Key Logic

| Node | Role |
|---|---|
| Victron input nodes | Reads live data directly from connected devices on the Pi |
| Join node | Holds all messages until a complete set of readings is available |
| Function node | Parses values, structures payload, sets measurement name |
| InfluxDB out | Writes the complete structured payload to InfluxDB |
