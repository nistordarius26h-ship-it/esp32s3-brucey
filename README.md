# ESP32-S3 Bruce Multi-Tool PCB

[![MCU](https://img.shields.io/badge/MCU-ESP32--S3-blue)](https://github.com/espressif/arduino-esp32)
[![Firmware](https://img.shields.io/badge/Firmware-Bruce-purple)](https://github.com/pr3y/Bruce)
[![PCB](https://img.shields.io/badge/PCB-Custom%20Carrier%20Board-orange)](#hardware)
[![License](https://img.shields.io/badge/License-MIT-red)](#license)

## Overview

Custom carrier PCB for the ESP32-S3, designed to run the [Bruce](https://github.com/pr3y/Bruce) firmware. Consolidates Bruce's Sub-GHz, 2.4GHz, NFC, and IR peripheral set onto a single board with shared I2C/SPI buses, local storage, onboard display, physical UI, and LiPo power management — built as a standalone handheld unit rather than a breadboard stack of breakout modules.

> **Disclaimer**
>
> Educational and research use only. Capture, analyze, or transmit signals only on hardware/networks you own or are authorized to test. RF and IR transmission are subject to jurisdiction-specific regulations — confirm local compliance before use.

---

## Design Summary

| Parameter | Value |
| --- | --- |
| MCU | ESP32-S3 |
| Firmware | Bruce (custom board profile / pinmap) |
| RF front ends | CC1101 (Sub-GHz), NRF24L01+ PA/LNA (2.4GHz) |
| NFC/RFID | PN532 V3 (I2C) |
| Display | 0.96" SSD1306 OLED, 128x64 (I2C) |
| Storage | MicroSD, SPI, onboard 5V→3.3V reg + level shifting |
| IR | KY-005 TX, KY-022/VS1838B RX (GPIO) |
| Input | 5x tactile buttons, active-low to GND |
| Power | TP4056 (USB-C) → 3.7V 2000mAh LiPo → SPDT power switch |
| Bus conditioning | I2C pull-ups (4.7kΩ x2), RF supply decoupling (10µF x2, 100nF x2) |

---

## Hardware

| Component | Interface | Function |
| --- | --- | --- |
| CC1101 Sub-GHz transceiver | 2x4 header, SPI | Sub-GHz capture/analysis/replay — fobs, garage/gate remotes |
| NRF24L01+ PA/LNA | 2x4 header, SPI | 2.4GHz sniffing — BLE advertisements, mousejacking, drone links |
| PN532 NFC module V3 | I2C (shared bus) | RFID/NFC read, write, emulation |
| SSD1306 OLED, 128x64 | I2C (shared bus) | System UI / menu rendering |
| MicroSD breakout | SPI, onboard reg + level shifter | Signal log / PCAP / config storage |
| IR TX (KY-005) | GPIO | IR remote replay / brute-force |
| IR RX (KY-022 / VS1838B) | GPIO | IR signal capture/decode |
| 5x tactile switches | GPIO, active-low | Up / Down / Left / Right / Select |
| TP4056 | USB-C | LiPo charge management, short-circuit protection |
| SPDT slide switch | — | Master power cutoff on TP4056 output rail |
| Decoupling network | — | 2x 10µF electrolytic + 2x 100nF ceramic at CC1101/NRF24 supply pins |
| I2C pull-ups | — | 2x 4.7kΩ on SDA/SCL to 3.3V rail |

---

## Firmware

Flash [Bruce](https://github.com/pr3y/Bruce) with pin definitions matched to this board's schematic. Custom board profile required — CC1101/NRF24 chip-select and IRQ lines, I2C bus assignment for OLED + PN532, MicroSD SPI pins, and button GPIO map are documented in `pcb/pinout.md`.

---

## Project Structure

```
firmware/            # Bruce board profile, pin definitions, build notes
pcb/                 # Schematic, layout, gerbers, BOM, pinout.md
media/               # Board photos, renders, bring-up shots
```

---

## Build Notes

- Populate RF headers (CC1101, NRF24) first, verify supply rails before adding digital peripherals.
- Shared I2C bus (OLED + PN532) — confirm address conflicts before flashing.
- MicroSD breakout's onboard regulator handles 5V→3.3V; do not feed 3.3V directly into its VCC pin.
- Bring-up order: power path (TP4056 → switch → rail) → MCU boot → OLED → SPI peripherals → RF modules.

---

## Future Work

- Upstream board profile submission to Bruce
- Enclosure design (3D printed) sized to button/OLED/USB-C cutouts
- Power draw characterization per peripheral, idle vs. active RF
- Battery life benchmarking under mixed workload

---

## License

MIT License

---

## Author

Nistor Darius
Embedded Systems / Wireless Research / Hardware Design
