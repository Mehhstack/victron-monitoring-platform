# Raspberry Pi — Edge Device Setup

## Overview

Each monitoring site runs a **Raspberry Pi** as the core edge device, replacing a dedicated
Victron Cerbo GX at a fraction of the cost. It runs VenusOS, communicates with Victron
devices, and integrates Pylontech batteries via a CAN HAT.

---

## Why Raspberry Pi Over a Cerbo GX?

| Feature | Raspberry Pi + VenusOS | Victron Cerbo GX |
|---|---|---|
| Cost | ~R725.70–R922.70 | ~R3400–R3900 |
| VenusOS support | ✅ Full | ✅ Native |
| Custom integrations | ✅ SSH, Node-RED, scripts | ⚠️ Limited |
| CAN HAT support | ✅ With HAT | ✅ Built-in |

---

## Hardware Used

| Component | Detail |
|---|---|
| Board | [Raspberry Pi Zero 2W](https://www.pishop.co.za/store/raspberry-pi-zero-2-wh-with-pre-soldered-header) |
| OS | VenusOS (Victron) |
| CAN Interface | [Waveshare RS485 CAN HAT](https://www.diyelectronics.co.za/store/hats/2364-rs485-can-hat-for-raspberry-pi.html?utm_campaign=google_shopping&utm_source=cpc&utm_medium=evergreen&utm_content=20485427402&gad_source=1&gad_campaignid=20489613595&gbraid=0AAAAADpm0I-TOvf0zgzj9XkgU3KdXbsJO&gclid=CjwKCAjw46HPBhAMEiwASZpLRJjuc576oQeG9BBj6ytX8fWCuS43gNeXfje1MNEHIwXW3vGT--fDbRoCao8QAvD_BwE) **Only needed if you want to connect VE.CAN devices** |
| Battery | Pylontech US2000 / US3000 / UPS2500 |
| Connectivity | [3 Port USB 2.0 Hub HAT with Ethernet](https://www.pishop.co.za/store/ethernet--usb-hub-hat-b-for-raspberry-pi-series-1x-rj45-3x-usb-20?keyword=3%20Port%20USB%202.0%20Hub%20HAT%20with%20Ethernet&category_id=0) |
| Power | 5V USB |
| Case | ![Acrylic Case With Heatsink](https://www.pishop.co.za/store/clear-acrylic-case-with-heatsink-for-raspberry-pi-zero) |
|SD Card | ![SanDisk Ultra 32GB](https://www.pishop.co.za/store/sandisk-ultra-microsdhc-32gb-uhs-i-100mbs-class10?keyword=SD%20card%2032gb&category_id=0) |

---

## VenusOS

VenusOS is Victron's open-source GX operating system — the same OS that runs on their
Cerbo GX. Victron officially supports it on Raspberry Pi, making the Pi a fully compatible
GX device.

### Installation

Download the latest VenusOS Large image from the [Victron Venus site](https://updates.victronenergy.com/feeds/venus/candidate/images/raspberrypi2/)

Use Balena Etcher / Raspberry Pi Imager.

### Key Settings After First Boot

- **Install Node-RED:**
- **Enable Super User and setup SSH** 

### Screenshots

> 📸 VenusOS Remote Console showing connected devices and system overview

![VenusOS Dashboard](https://github.com/Mehhstack/victron-monitoring-platform/blob/main/Raspberry%20Pi/VenusOS%20dash.png)

---

## Pylontech Battery Integration via CAN HAT

Pylontech batteries communicate over **CAN bus**. A CAN HAT adds this interface to the Pi,
allowing VenusOS to read SOC, voltage, current, cell temps, and charge/discharge limits.

### Wiring

Victron has a dedicated ![guide](https://www.victronenergy.com/live/battery_compatibility:pylontech_phantom) on how to setup different models of the pylontech batteries for you victron system. Choose the right battery and correct wiring other wise the battery will not show up on the device list. 

Pylontech uses an RJ45 CAN port — same connector as Ethernet but **not** Ethernet.
Do not connect to a network switch.

This is for the **Pylontech UP2500**

| Function | Victron VE.CAN side | CAN HAT Terminal |
|---|---|---|
| GND | RJ45 Pin 3 | GND |
| CAN-L | RJ45 Pin 8 | CAN-L |
| CAN-H | RJ45 Pin 7 | CAN-H |

### CAN Interface Configuration

```bash
nano /u-boot/config.txt
[all]
dtparam=spi=on
dtoverlay=mcp2515-can0,oscillator=12000000,interrupt=25,spimaxfrequency=2000000
```

> Pylontech CAN bitrate is **500 kbps**.

![VenusOS CAN configure](https://github.com/Mehhstack/victron-monitoring-platform/blob/main/Raspberry%20Pi/VenusOS%20CAN-bus.png)

### Screenshots

> 📸 CAN HAT installed on Raspberry Pi with RJ45 CAN cable connected to Pylontech battery

![CAN HAT Installed](screenshots/can-hat-installed.png)

> 📸 VenusOS showing Pylontech battery detected with SOC, voltage, and cell data

![Pylontech Detected](https://github.com/Mehhstack/victron-monitoring-platform/blob/main/Raspberry%20Pi/pylontech%20with%20VenusOS.png)

---

## Hardware Setup

> 📸 Raspberry Pi with CAN HAT — as deployed on site

![Pi Hardware Setup](https://github.com/Mehhstack/victron-monitoring-platform/blob/main/Raspberry%20Pi/Raspberry%20Pi%20with%20HAT.jpg)
