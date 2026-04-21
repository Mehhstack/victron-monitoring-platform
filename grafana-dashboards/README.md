# Grafana Dashboards

## Purpose
:contentReference[oaicite:0]{index=0} is used as the visualization and alerting layer for the monitoring platform.

It provides:
- A unified dashboard for all solar tower sites
- Individual dashboards per site
- Real-time visibility into power and system health
- Alerting based on thresholds and failures

---

## Dashboards

### 1. Global Overview Dashboard
Displays all sites in a single view:
- Battery voltage per site
- Load usage
- System status (online/offline)
- Alerts summary

![Global Dashboard](../screenshots/global-dashboard.png)

---

### 2. Per-Site Dashboard
Detailed metrics for a single tower:
- Solar yield
- Battery voltage and state
- Load consumption
- Historical trends

![Site Dashboard](../screenshots/site-dashboard.png)

---

## Alerting
Alerts are configured for:
- Low battery voltage
- High load usage
- Device offline

Alerts can be sent via:
- Email
- Telegram (optional)
- Other integrations

---

## Notes
Dashboards are designed for:
- Quick fault detection
- Low-bandwidth environments
- Operational visibility across distributed infrastructure
