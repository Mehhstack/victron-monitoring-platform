# Grafana Dashboards

## Purpose

Grafana is the visualisation and alerting layer of the platform. It connects directly
to InfluxDB and turns raw telemetry into real-time dashboards for every site, as well
as a unified view across all sites from a single screen.

Because everything is locally hosted, Grafana remains fully accessible even if the
Victron VRM site or internet connection goes down.

---

## Dashboards

### 1. Global Overview Dashboard

A single unified view across all monitored sites. Useful for quickly spotting which
sites need attention without clicking into each one individually.

Displays:
- Battery voltage per site
- Solar production per site
- SOC of batteries per site
- Daily yield of solar per site
- Active alerts summary

![Global Dashboard](victron%20unified%20dash.png)

---

### 2. Per-Site Dashboard

Detailed drill-down for a single site. Used for investigating issues, reviewing
historical trends, and verifying system health after changes.

Displays:
- Solar yield and PV power output
- Battery voltage, current, and state of charge
- Load consumption over time
- Historical trend graphs
- Charger state (Bulk / Absorption / Float)

![Per-Site Dashboard](victron%20per%20site%20dash.png)

---

## Alerting

Alerts are evaluated against InfluxDB data on a schedule and fire when thresholds
are breached.

| Alert | Condition |
|---|---|
| Low battery voltage | Voltage drops below defined threshold |
| High load usage | Load exceeds safe operating limit |
| Device offline | No data received from site within timeout window |
| Solar charger fault | Charger state indicates fault or disconnect |

### Notification Channels

| Channel | Status |
|---|---|
| Email | ✅ Configured |
| Telegram | ✅ Optional |
| Other webhooks | ✅ Supported via Grafana contact points |
