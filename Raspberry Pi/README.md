# Raspberry Pi — Edge Device Setup

## Overview

Each monitoring site runs a **Raspberry Pi** as the core edge device, replacing a dedicated
Victron Cerbo GX at a fraction of the cost. It runs VenusOS, communicates with Victron
devices, and integrates Pylontech batteries via a CAN HAT.

---

## Why Raspberry Pi Over a Cerbo GX?

| Feature | Raspberry Pi + VenusOS | Victron Cerbo GX |
|---|---|---|
| Cost | ~$50–80 | ~$200–250 |
| VenusOS support | ✅ Full | ✅ Native |
| Custom integrations | ✅ SSH, Node-RED, scripts | ⚠️ Limited |
| CAN HAT support | ✅ With HAT | ✅ Built-in |

---

## Hardware Used

| Component | Detail |
|---|---|
| Board | Raspberry Pi 4 (2GB or 4GB) |
| OS | VenusOS (Victron) |
| CAN Interface | Waveshare RS485 CAN HAT or compatible |
| Battery | Pylontech US2000 / US3000 |
| Connectivity | Ethernet (preferred) |
| Power | 5V USB-C or DIN rail PSU |

---

## VenusOS

VenusOS is Victron's open-source GX operating system — the same OS that runs on their
Cerbo GX. Victron officially supports it on Raspberry Pi, making the Pi a fully compatible
GX device.

### Installation

Download the latest image from the [Victron Venus GitHub](https://github.com/victronenergy/venus)
and flash it to an SD card:

```bash
sudo dd if=venus-image-raspberrypi4.wic.gz of=/dev/sdX bs=4M status=progress
```

Or use Balena Etcher / Raspberry Pi Imager.

### Key Settings After First Boot

- **Node-RED:** Settings → Venus OS Large → Install large image
- **MQTT:** Settings → Services → MQTT on LAN → Enable
- **SSH:** Settings → General → SSH → Enable

### Screenshots

> 📸 VenusOS Remote Console showing connected devices and system overview

![VenusOS Dashboard](screenshots/venus-dashboard.png)

---

## Pylontech Battery Integration via CAN HAT

Pylontech batteries communicate over **CAN bus**. A CAN HAT adds this interface to the Pi,
allowing VenusOS to read SOC, voltage, current, cell temps, and charge/discharge limits.

### Wiring

Pylontech uses an RJ45 CAN port — same connector as Ethernet but **not** Ethernet.
Do not connect to a network switch.

| RJ45 Pin | Signal | CAN HAT Terminal |
|---|---|---|
| Pin 1 | CAN-H | CAN H |
| Pin 2 | CAN-L | CAN L |
| Pin 7 | GND | GND |

### CAN Interface Configuration

```bash
auto can0
iface can0 inet manual
    pre-up /sbin/ip link set can0 type can bitrate 500000
    up /sbin/ifconfig can0 up
    down /sbin/ifconfig can0 down
```

> Pylontech CAN bitrate is **500 kbps**.

### Verify CAN is Working

```bash
ip link show can0
candump can0    # Should show frames from Pylontech if wired correctly
```

Once active, VenusOS auto-detects the battery:
Settings → System Setup → Battery Monitor → Select Pylontech

### Screenshots

> 📸 CAN HAT installed on Raspberry Pi with RJ45 CAN cable connected to Pylontech battery

![CAN HAT Installed](screenshots/can-hat-installed.png)

> 📸 VenusOS showing Pylontech battery detected with SOC, voltage, and cell data

![Pylontech Detected](screenshots/pylontech-detected.png)

---

## Hardware Setup

> 📸 Raspberry Pi with CAN HAT — as deployed on site

![Pi Hardware Setup](screenshots/pi-hardware-setup.png)

---

## Notes

- Use a quality SD card (Samsung Endurance Pro or similar) — VenusOS writes frequently
- Ethernet is strongly preferred over WiFi for reliability on site
- If stacking multiple Pylontech batteries, set one as **master** using the DIP switches
- VRM connectivity is optional — everything runs fully offline without it
- Label all cables at installation — saves time during troubleshooting
