# Node-RED Flows

## Purpose
:contentReference[oaicite:1]{index=1} runs on the Victron GX device and is responsible for collecting and forwarding telemetry data.

It acts as the bridge between Victron systems and the central database.

---

## Functionality

- Collects data from Victron system:
  - Battery voltage
  - Load
  - Solar production
- Formats telemetry data
- Sends data to :contentReference[oaicite:2]{index=2}

---

## Data Flow

Victron GX → Node-RED → InfluxDB

---

## Example Flows

![Node-RED Flows](https://github.com/Mehhstack/victron-monitoring-platform/blob/main/node-red/node-red%20battery%20flow.png)
![Node-RED Flows](https://github.com/Mehhstack/victron-monitoring-platform/blob/main/node-red/node-red%20solar%20charger%20flow.png)
