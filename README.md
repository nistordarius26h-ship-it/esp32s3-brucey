# ESP32-S3 Brucey

Custom ESP32-S3 handheld built around Bruce firmware.

I originally made this as a carrier PCB for an ESP32-S3 and a few external modules. After building the board I also started adapting Bruce for the hardware, mainly because I wanted everything on one compact device instead of using jumper wires and separate modules.

This is still a DIY prototype. The hardware is assembled and tested, the firmware boots and the basic OLED interface works, but not every Bruce feature is fully adapted yet.

## Hardware

- ESP32-S3 N16R8
  - 16 MB Flash
  - 8 MB PSRAM
- SSD1306 0.96" 128x64 OLED
- CC1101 Sub-GHz module
- NRF24L01+ 2.4 GHz module
- PN532 NFC/RFID module
- MicroSD reader
- IR transmitter
- IR receiver
- 5 navigation buttons
- LiPo battery
- TP4056 charging board
- Power switch
- 2 external antennas
- Local decoupling for the RF modules

## Pinout

### I2C

| Function | GPIO |
|---|---:|
| SDA | 17 |
| SCL | 18 |

Used by the SSD1306 and PN532.

### Shared SPI

| Function | GPIO |
|---|---:|
| MOSI | 11 |
| MISO | 13 |
| SCK | 12 |

### CC1101

| Function | GPIO |
|---|---:|
| CS | 10 |
| GDO0 | 2 |

### NRF24L01+

| Function | GPIO |
|---|---:|
| CSN | 14 |
| CE | 15 |

### MicroSD

| Function | GPIO |
|---|---:|
| CS | 46 |

### IR

| Function | GPIO |
|---|---:|
| TX | 9 |
| RX | 8 |

### Buttons

| Button | GPIO |
|---|---:|
| Up | 1 |
| Down | 4 |
| Left | 5 |
| Right | 6 |
| Select | 7 |

The buttons are active-low and use the ESP32 internal pull-ups.

Current button mapping:

- Up: previous item
- Down: next item
- Left: back
- Right: select
- Middle: select

## Firmware

The firmware is based on [Bruce](https://github.com/BruceDevices/firmware).

This board is not an official Bruce target, so I made a custom configuration for it. The changes include:

- custom board configuration
- GPIO and pin definitions
- button handling
- runtime module pin configuration
- SSD1306 initialization
- SSD1306 menu rendering

Bruce normally targets larger TFT displays. This build uses a 128x64 monochrome SSD1306, so some parts of the interface still need to be adapted.

The main menu and submenus work, but some tools still use TFT-specific drawing code.

I am better at the hardware side than the software side, so the firmware is something I am still learning and changing. If you build this project, expect to modify the software depending on what you want from it.

## Current status

Working / tested:

- ESP32-S3 boots correctly
- 16 MB Flash configured
- 8 MB PSRAM detected
- SSD1306 working
- Bruce main menu working on OLED
- navigation buttons working
- MicroSD mounts correctly
- I2C working on GPIO17 / GPIO18
- CC1101 runtime pins configured
- NRF24 runtime pins configured
- IR pins configured
- LiPo charging
- power switch
- mechanical assembly

Still being tested / unfinished:

- NRF24 currently reports `NRF24 not found`
- some Bruce tools still use TFT-specific screens
- some feature pages need SSD1306-specific display code
- not every Bruce function has been tested
- battery-only power needs improvement

## Power

The current board uses:

```text
LiPo
  |
TP4056
  |
Power switch
  |
ESP32-S3 VIN / 5V
```

This works for testing, but a single-cell LiPo does not provide a real 5 V rail.

During testing the 3.3 V rail dropped too low when running from the battery only. The board and OLED work much better when USB power is connected.

A simple improvement is to add a small 5 V boost converter:

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

That should give the onboard regulator enough headroom and improve stability, especially with the RF modules active.

A future PCB revision could use a proper 3.3 V buck-boost supply instead.

## Build

The firmware is built with PlatformIO.

Build:

```bash
pio run -e bruce-handheld-s3
```

Upload:

```bash
pio run -e bruce-handheld-s3 -t upload
```

Serial monitor:

```bash
pio device monitor -b 115200
```

## Notes

The CC1101, NRF24 and MicroSD share the same SPI lines and use separate chip-select pins.

The PN532 is used in I2C mode together with the SSD1306.

Current NRF24 configuration:

```text
SCK  = 12
MISO = 13
MOSI = 11
CSN  = 14
CE   = 15
```

Current CC1101 configuration:

```text
SCK   = 12
MISO  = 13
MOSI  = 11
CS    = 10
GDO0  = 2
```

Current MicroSD configuration:

```text
SCK  = 12
MISO = 13
MOSI = 11
CS   = 46
```

## Project status

This is a working prototype, not a finished product.

The hardware side is mostly done. The main work left is firmware cleanup, testing the external modules and adapting more of Bruce's TFT-oriented screens to the SSD1306.

## Intended use

Made for electronics, embedded development, RF/NFC/IR experiments and learning.

Use wireless and security-related functions only on systems and devices you own or have permission to test.

## Credits

Bruce firmware:

https://github.com/BruceDevices/firmware

## Disclaimer

Experimental DIY project. Use at your own risk.
