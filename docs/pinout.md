# Pinout

Pinout for my custom ESP32-S3 Brucey handheld.

The ESP32-S3, CC1101, NRF24L01+ and MicroSD share the same SPI bus.  
The SSD1306 OLED and PN532 share the same I2C bus.

## ESP32-S3

Main controller:

- ESP32-S3 N16R8
- 16 MB Flash
- 8 MB PSRAM

## I2C Bus

| Function | GPIO |
|---|---:|
| SDA | 17 |
| SCL | 18 |

Used by:

- SSD1306 OLED
- PN532 NFC/RFID

The board uses external 4.7 kΩ pull-up resistors from SDA and SCL to 3.3 V.

## Shared SPI Bus

| Function | GPIO |
|---|---:|
| MOSI | 11 |
| MISO | 13 |
| SCK | 12 |

Used by:

- CC1101
- NRF24L01+
- MicroSD

Each SPI device uses its own chip-select pin.

## CC1101

| Function | GPIO |
|---|---:|
| SCK | 12 |
| MISO | 13 |
| MOSI | 11 |
| CS | 10 |
| GDO0 | 2 |

Power:

- VCC -> 3.3 V
- GND -> GND

## NRF24L01+

| Function | GPIO |
|---|---:|
| SCK | 12 |
| MISO | 13 |
| MOSI | 11 |
| CSN | 14 |
| CE | 15 |

Power:

- VCC -> 3.3 V
- GND -> GND

I added local decoupling close to the NRF24 module:

- 100 nF ceramic capacitor
- 10 uF electrolytic capacitor

## MicroSD

| Function | GPIO |
|---|---:|
| SCK | 12 |
| MISO | 13 |
| MOSI | 11 |
| CS | 46 |

Power:

- VCC -> switched power rail
- GND -> GND

GPIO46 is used as the SD chip-select on this PCB.

## SSD1306 OLED

| Function | GPIO |
|---|---:|
| SDA | 17 |
| SCL | 18 |

Display:

- SSD1306
- 0.96 inch
- 128x64
- I2C
- Typical address: 0x3C

Power:

- VCC -> 3.3 V
- GND -> GND

## PN532

The PN532 is used in I2C mode.

| Function | GPIO |
|---|---:|
| SDA | 17 |
| SCL | 18 |

PN532 mode:

- Switch 1: ON
- Switch 2: OFF

Power:

- VCC -> 3.3 V
- GND -> GND

## IR

| Function | GPIO |
|---|---:|
| IR TX | 9 |
| IR RX | 8 |

Modules used:

- KY-005 IR transmitter
- KY-022 IR receiver

## Buttons

All buttons are active-low and are connected to GND when pressed.

The firmware uses the ESP32 internal pull-ups.

| Button | GPIO | Current action |
|---|---:|---|
| Up | 1 | Previous item |
| Down | 4 | Next item |
| Left | 5 | Back |
| Right | 6 | Select / Enter |
| Middle | 7 | Select / Enter |

## Power

Current power path:

```text
LiPo +
  |
TP4056 B+
TP4056 OUT+
  |
Power switch
  |
ESP32-S3 VIN / 5V
```

LiPo negative and TP4056 negative are connected to common GND.

The current design does not generate a real 5 V rail from the single-cell LiPo.

A recommended upgrade is:

```text
LiPo
  |
TP4056
  |
Power switch
  |
5 V boost converter
  |
ESP32-S3 VIN / 5V
```

This should improve power stability when running from battery, especially with the RF modules active.

## Quick Reference

| Device | SCK | MISO | MOSI | CS / CSN | Extra |
|---|---:|---:|---:|---:|---:|
| CC1101 | 12 | 13 | 11 | 10 | GDO0 = 2 |
| NRF24L01+ | 12 | 13 | 11 | 14 | CE = 15 |
| MicroSD | 12 | 13 | 11 | 46 | - |

| Device | SDA | SCL |
|---|---:|---:|
| SSD1306 | 17 | 18 |
| PN532 | 17 | 18 |

| Function | GPIO |
|---|---:|
| IR TX | 9 |
| IR RX | 8 |
| Up | 1 |
| Down | 4 |
| Left | 5 |
| Right | 6 |
| Middle | 7 |

## Notes

- CC1101, NRF24 and MicroSD share the same SPI lines.
- Keep inactive SPI chip-select pins HIGH.
- SSD1306 and PN532 share I2C on GPIO17 / GPIO18.
- The NRF24 is currently configured correctly in firmware but still needs hardware troubleshooting because it reports `NRF24 not found`.
- The MicroSD has been tested and mounts successfully.
