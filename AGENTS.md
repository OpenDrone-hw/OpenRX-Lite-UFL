# OpenRX Lite-UFL

Open-source 2.4 GHz ExpressLRS receiver. An ESP32-C3 controls an SX1281 radio;
the ELRS link terminates in a U.FL connector, while a separate ceramic antenna
serves the ESP32-C3 Wi-Fi interface.

## Architecture

The ESP32-C3 (U1) runs ExpressLRS, communicates with the flight controller over
UART0/CRSF, and drives the WS2812B status LED (D1). The SX1281 (U3) uses SPI and
a 52 MHz TCXO. Its 2.4 GHz RFIO passes through the 2450FM07D0034T filter (FL1)
to U.FL connector J1; there is no PA, LNA or RF switch. The
2450AT18A100E antenna (AE1, net `WIFI`) belongs only to the ESP32-C3 Wi-Fi
interface used for flashing and configuration.

## Power

```text
5V pad (TP3)
└── TLV75533PDQNR (U2), 3.3 V
    ├── ESP32-C3 (U1) and status LED (D1)
    └── SX1281 (U3) and 52 MHz TCXO (OSC1)
```

## Key parts

| Function | Ref | Part | LCSC | Note |
|---|---|---|---|---|
| MCU | U1 | ESP32-C3 | | QFN-32 |
| 3.3 V LDO | U2 | TLV75533PDQNR | C2861882 | 500 mA |
| 2.4 GHz radio | U3 | SX1281IMLTRT | C2151551 | |
| 2.4 GHz filter | FL1 | 2450FM07D0034T | C2651081 | |
| ELRS connector | J1 | U.FL-R-SMT-1(80) | C88374 | 50 ohm |
| Wi-Fi antenna | AE1 | 2450AT18A100E | C89334 | ESP32-C3 only |
| Radio TCXO | OSC1 | OW7EL89CENUNFAYLC-52M | C22434896 | 52 MHz |
| MCU crystal | X1 | CJ17-400001010B20 | C2875272 | 40 MHz |
| Status LED | D1 | XL-1010RGBC-WS2812B | C5349953 | |

## Connectors and I/O

| Pad | Net | ESP32-C3 | Function |
|---|---|---|---|
| RX (TP1) | U0RXD | GPIO20 | CRSF serial input |
| TX (TP2) | U0TXD | GPIO21 | CRSF telemetry output |
| 5V (TP3) | +5V | - | Supply input |
| GND (TP4) | GND | - | Ground |
| BOOT (TP5) | BOOT | GPIO9 | UART download mode when held low at power-up |

Radio SPI is SCK/MOSI/MISO on GPIO6/4/5; NSS/RST/BUSY/DIO1 are GPIO7/2/3/1.
The RGB status LED is GPIO8 in GRB order.

## Layout rules

Preserve the short 50-ohm SX1281-to-filter-to-U.FL path, its ground-via fence,
and the Wi-Fi antenna keepout. Keep the external ELRS antenna away from the
on-board Wi-Fi antenna in the product installation.

## Firmware

ExpressLRS target `Unified_ESP32C3_2400_RX`, platform `esp32-c3`, minimum
version 3.5.0. Upload methods are UART, Wi-Fi and Betaflight passthrough. The
authoritative GPIO and power values are in
`firmware/OpenRX Lite-UFL 2400.json`.

## Repo

| | |
|---|---|
| Maintainer | @bastian2001 |
| KiCad project | `hardware/OpenRX-Lite-UFL.kicad_pro` |
| Root schematic | `hardware/OpenRX-Lite-UFL.kicad_sch`; radio/core sheet `hardware/esp32c3_sx1281_lite.kicad_sch` |
| Board | `hardware/OpenRX-Lite-UFL.kicad_pcb`, 6 layers, 1.0 mm |
| Local library | `hardware/lib.pretty/` and `hardware/lib.3dshapes/`, nickname `OpenRX-Shared` retained by the design; `shared/libs/OpenRX-Shared.3dshapes` is a compatibility link for embedded legacy model paths |
| Design rules | `hardware/OpenRX-Lite-UFL.kicad_dru` |
| Firmware target | `firmware/OpenRX Lite-UFL 2400.json` and `firmware/targets_entries.json` |

## Rules

The shared [Board rules](https://github.com/OpenDrone-hw/hardware-template/blob/main/AGENTS.md#board-rules) apply. Only what differs for this board is listed here.

## Revisions

| Rev | Date | Change |
|---|---|---|
| rev2 | 2026-08-14 | Fab set re-exported and verified; U.FL LCSC field corrected. |
| rev1 | 2026-03-23 | First released OpenRX Lite-UFL design. |

