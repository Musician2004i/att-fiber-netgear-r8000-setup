# att-fiber-netgear-r8000-setup
A comprehensive configuration guide for maximizing AT&amp;T Fiber throughput, reducing latency, and configuring IP Passthrough with the Netgear Nighthawk X6 (R8000).

# The Unthrottled Home Network: AT&T Fiber & Netgear R8000 Setup

![License](https://img.shields.io/badge/license-MIT-blue.svg) ![Status](https://img.shields.io/badge/status-active-success.svg)

## 📖 Overview
This repository documents the specific configurations required to maximize the performance of an **AT&T Fiber** connection when bypassing the ISP-provided routing functions in favor of a **Netgear Nighthawk X6 (R8000)**.

The goal is to eliminate double NAT, reduce bufferbloat, and ensure the R8000 handles all routing logic for the home lab and LAN.

## 🏗 Architecture
The physical topology for this setup:

`[ISP Fiber Jack]` -> `[AT&T Gateway (BGW320/BGW210)]` -> `[Netgear R8000]` -> `[Switch/Devices]`



[Image of home network diagram]


## ⚙️ Hardware
* **ISP Gateway:** AT&T BGW320-500 (or BGW210)
* **Personal Router:** Netgear Nighthawk X6 (R8000) AC3200
* **Cabling:** Cat6 or Cat6a Patch Cables

## 🚀 Configuration Steps

### 1. AT&T Gateway Settings (IP Passthrough)
To ensure the R8000 receives the public WAN IP directly:
1.  Navigate to `192.168.1.254`.
2.  Go to **Firewall** > **Packet Filter** -> **Disable Packet Filters**.
3.  Go to **Firewall** > **IP Passthrough**.
4.  **Allocation Mode:** Passthrough.
5.  **Passthrough Mode:** DHCPS-fixed.
6.  **Passthrough Fixed MAC Address:** [Select your R8000 MAC Address].
7.  **Wi-Fi:** Turn off both 2.4GHz and 5GHz radios on the AT&T unit to reduce interference.

### 2. Netgear R8000 Configuration
* **Internet Setup:** Set to "Get Automatically from ISP."
* **MTU Size:** 1500 (Standard for Fiber).
* **QoS:** Disabled (Often throttles Gigabit speeds on older firmware) OR Dynamic QoS enabled (if focusing on bufferbloat management).
* **DNS:** Manually set to Cloudflare (`1.1.1.1`) or Google (`8.8.8.8`) for faster lookups.

### 3. Optimization Checks
- [ ] Disable "SIP ALG" on the Netgear R8000 (Crucial for VoIP/Zoom calls).
- [ ] Verify IPv6 settings (Pass-through vs Native).
- [ ] Test for Double NAT (Run `tracert 8.8.8.8` - you should only see one local hop).

## 📊 Performance Results
* **Down:** ~940 Mbps
* **Up:** ~940 Mbps
* **Ping:** < 5ms

## 📝 Notes
* *Keep the AT&T Gateway subnet (usually 192.168.1.x) different from the Netgear subnet (e.g., set Netgear to 192.168.10.1) to avoid IP conflicts.*

## 🤝 Contributing
Feel free to submit pull requests if you have found better settings for the R8000 or custom firmware scripts (DD-WRT/FreshTomato).
