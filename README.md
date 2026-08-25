# ESP32-S3 Bruce Multi-Tool PCB

[![Arduino](https://img.shields.io/badge/Arduino-ESP32S3-blue)](https://github.com/espressif/arduino-esp32)
[![Firmware](https://img.shields.io/badge/Firmware-Bruce-purple)](https://github.com/pr3y/Bruce)
[![Wireless](https://img.shields.io/badge/Wireless-SubGHz%2FBLE%2FNFC%2FIR-green)](#hardware)
[![Research](https://img.shields.io/badge/Research-Embedded%20Security-orange)](#overview)
[![License](https://img.shields.io/badge/License-MIT-red)](#license)

## Overview

This project is a **custom carrier PCB for the [Bruce](https://github.com/pr3y/Bruce) firmware**, built around an ESP32-S3 module. Bruce is an open-source multi-tool firmware for sniffing, capturing, and analyzing Sub-GHz, 2.4GHz, NFC, and IR signals — this board brings all of its supported peripherals together on a single, purpose-built handheld PCB with its own display, storage, buttons, and battery management.

> **Disclaimer**
>
> This project is intended for educational and research purposes only. Only capture, analyze, or transmit signals on devices/networks you own or have explicit permission to test. Sub-GHz, RFID, and IR transmission/reception may be restricted or illegal depending on your jurisdiction — check local regulations before use.

---

## Features

- ESP32-S3 core running the Bruce multi-tool firmware
- Sub-GHz sniffing/transmit via CC1101 (car fobs, garage doors, remotes)
- 2.4GHz sniffing via NRF24L01+ PA/LNA (BLE advertisements, mousejacking, drones)
- NFC/RFID read, write, and emulation via PN532 over I2C
- 0.96" SSD1306 OLED for onboard menu/UI
- MicroSD storage for captured logs, PCAPs, and configs
- IR transmit (KY-005) and receive (KY-022 / VS1838B) for remote cloning/playback
- 5-button navigation (Up/Down/Left/Right/Select)
- TP4056 USB-C LiPo charging + physical power switch
- Onboard decoupling/pull-up network for clean shared I2C and RF power rails

---

## Technologies

- C++ (Arduino IDE / ESP-IDF via Bruce firmware)
- I2C (OLED + PN532), SPI (CC1101, NRF24L01+, MicroSD)

---

## Hardware

| Component | Interface | Purpose |
| --- | --- | --- |
| ESP32-S3 module | — | Main MCU running Bruce firmware |
| CC1101 Sub-GHz transceiver | 2x4 header (SPI) | Sniff/capture/transmit Sub-GHz (fobs, garage doors, remotes) |
| NRF24L01+ PA/LNA | 2x4 header (SPI) | 2.4GHz sniffing — BLE advertisements, mousejacking, drones |
| PN532 NFC module (V3) | I2C (shared bus) | Read/write/emulate RFID tags, badges, NFC credentials |
| 0.96" OLED (SSD1306, 128x64) | I2C (shared bus) | Onboard menu / system UI |
| MicroSD breakout (w/ 5V→3.3V reg + level shifter) | SPI | Stores raw signal logs, PCAPs, configs/scripts |
| IR transmitter (KY-005) | GPIO | Clone/playback/brute-force TV, AC, appliance remotes |
| IR receiver (KY-022 / VS1838B) | GPIO | Capture/decode remote signals in real time |
| 5x tactile buttons (active-low) | GPIO | Up / Down / Left / Right / Select |
| TP4056 charge module | USB-C | Charging + protection for 3.7V 2000mAh LiPo |
| 3-pin SPDT slide switch | — | Master power on/off (intercepts TP4056 output) |
| 2x 10µF electrolytic + 2x 100nF ceramic caps | — | Decoupling/filtering at NRF24 and CC1101 headers |
| 2x 4.7kΩ pull-ups | — | I2C_SDA / I2C_SCL bus pull-ups to 3.3V |

---

## Project Structure

```
firmware/            # Bruce firmware config/build notes
pcb/                 # Schematic, layout, and BOM files
media/               # Board photos, renders, screenshots
```

---

### Hardware

Assemble the custom PCB using the schematic and BOM located in the `pcb/` directory. Populate the CC1101 and NRF24L01+ headers, PN532 and OLED on the shared I2C bus, MicroSD breakout on SPI, IR TX/RX pair, navigation buttons, and TP4056/battery/power-switch chain per the schematic.

### Firmware

Flash the [Bruce](https://github.com/pr3y/Bruce) firmware to the ESP32-S3 module, matching the pin definitions to this board's schematic (custom board profile in Bruce's config).

---

## Future Work

- Custom Bruce board profile/pinmap submission upstream
- 3D-printed enclosure for the handheld form factor
- Battery life/power draw testing across peripherals
- Companion case cutouts for OLED/buttons/USB-C

---

## License

MIT License

---

## Author

Nistor Darius

Embedded Systems • Wireless Research • Hardware Design
