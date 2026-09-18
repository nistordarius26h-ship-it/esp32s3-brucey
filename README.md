# ESP32-S3 Brucey

Custom ESP32-S3 N16R8 handheld with SSD1306 OLED, CC1101, NRF24L01+, PN532, MicroSD and IR.

![Brucey assembled hardware](media/brucey_final_build.jpg)

The current firmware in this repository is the hardware-specific ESP-HACK port used on this device.

## Hardware

- ESP32-S3 N16R8 — 16 MB flash / 8 MB Octal PSRAM
- SSD1306 128x64 OLED
- CC1101
- NRF24L01+
- PN532 in I2C mode
- MicroSD
- KY-005 IR TX / KY-022 IR RX
- 5 active-low buttons
- LiPo + TP4056/protection + 5.1 V boost

## Current pinout

| Device / signal | GPIO |
|---|---:|
| I2C SDA | 17 |
| I2C SCL | 18 |
| RF SCK | 12 |
| RF MOSI | 11 |
| RF MISO | 13 |
| CC1101 CS | 10 |
| CC1101 GDO0 | 2 |
| NRF24 CSN | 14 |
| NRF24 CE | 15 |
| SD SCK | 12 |
| SD MOSI | 11 |
| SD MISO | 3 |
| SD CS | 46 |
| IR TX | 9 |
| IR RX | 8 |
| Up | 1 |
| Down | 4 |
| Left / Back | 5 |
| Right | 6 |
| Middle / OK | 7 |

The SD MISO was moved from GPIO13 to GPIO3 because the SD breakout did not release MISO correctly when deselected.

PN532 uses the same I2C bus as the OLED. PN532 mode is switch 1 ON / switch 2 OFF; the module is normally seen at address `0x24`.

## Firmware

Current compact source archive:

```text
firmware/ESP-HACK-BRUCEY-S3-v24.3-compact-final.zip
```

PlatformIO environment:

```text
BRUCEY_S3_SSD1306
```

The IR menu includes separate normal and repeated modes:

```text
Send
Read
TV-B-Gone
Remote
Send 3x
TV-B-Gone 3x
```

Normal `Send` transmits once. `Send 3x` repeats three times. Normal `TV-B-Gone` sends each power code once; `TV-B-Gone 3x` sends each code three times.

## Confirmed hardware tests

Raw SD initialization on the GPIO3 MISO wiring returned:

```text
CMD0 = 0x01
CMD8 = 0x01
R7   = 00 00 01 AA
```

CC1101 direct SPI diagnostics return version `0x14`.

NRF24 direct SPI diagnostics return valid status/address-width/channel register responses.

## Power

Working battery path:

```text
LiPo -> TP4056/protection -> 5.1 V boost -> ESP32 VIN/5V
```

The SD module is powered correctly from the battery/boost path. On the current board revision its supply was measured near zero in the USB-only state, so USB-only SD operation is not considered valid.

Do not assume the USB 5 V rail and boost output can be tied together safely unless the power path is isolated.

## Build

Extract the firmware archive, then:

```bash
pio run -e BRUCEY_S3_SSD1306
pio run -e BRUCEY_S3_SSD1306 -t upload
pio device monitor -b 115200
```

## Repository layout

- `firmware/ESP-HACK-BRUCEY-S3-v24.3-compact-final.zip` — current firmware source
- `docs/pinout.md` — current wiring
- `pcb/` — PCB manufacturing files
- `media/brucey_final_build.jpg` — assembled hardware photo
- `media/` — additional project images

This remains a DIY prototype; firmware behavior should be validated on the actual hardware after each update.
