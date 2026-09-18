# Brucey Pinout — Current Hardware

## ESP32-S3

- ESP32-S3 N16R8
- 16 MB flash
- 8 MB Octal PSRAM

## I2C

| Signal | GPIO |
|---|---:|
| SDA | 17 |
| SCL | 18 |

Used by SSD1306 and PN532.

PN532 I2C mode: switch 1 ON, switch 2 OFF. Address normally `0x24`.

## RF SPI

| Signal | GPIO |
|---|---:|
| SCK | 12 |
| MOSI | 11 |
| MISO | 13 |

### CC1101

| Signal | GPIO |
|---|---:|
| CS | 10 |
| GDO0 | 2 |

### NRF24L01+

| Signal | GPIO |
|---|---:|
| CSN | 14 |
| CE | 15 |

## MicroSD

| Signal | GPIO |
|---|---:|
| SCK | 12 |
| MOSI | 11 |
| MISO | 3 |
| CS | 46 |

SD MISO is intentionally separate from RF MISO.

## IR

| Signal | GPIO |
|---|---:|
| TX | 9 |
| RX | 8 |

## Buttons

| Button | GPIO |
|---|---:|
| Up | 1 |
| Down | 4 |
| Left / Back | 5 |
| Right | 6 |
| Middle / OK | 7 |

All buttons are active-low using internal pull-ups.

## Quick reference

| Device | SCK | MISO | MOSI | CS / CSN | Extra |
|---|---:|---:|---:|---:|---:|
| CC1101 | 12 | 13 | 11 | 10 | GDO0=2 |
| NRF24L01+ | 12 | 13 | 11 | 14 | CE=15 |
| MicroSD | 12 | 3 | 11 | 46 | - |

## Power

Current battery path:

```text
LiPo -> TP4056/protection -> 5.1 V boost -> ESP32 VIN/5V
```

The SD module has been confirmed working from battery/boost power. USB-only does not currently provide the SD module with its normal supply voltage.
