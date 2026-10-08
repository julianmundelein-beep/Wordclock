# Schematic

![Schematic](schaltplan.svg)

| File | Contents |
| --- | --- |
| [`schaltplan.svg`](schaltplan.svg) / [`schaltplan.png`](schaltplan.png) | Clean schematic (vector / raster) |
| [`schaltplan_skizze.png`](schaltplan_skizze.png) | Original hand-drawn sketch with parts list |
| [`bom.csv`](bom.csv) | Bill of materials |

The schematic was redrawn by hand as SVG from the sketch; there is no EDA source file.

## How it works

- **Power:** The 5 V supply (J1) feeds a common +5 V rail for the ESP, the level shifter and the LEDs via fuse F1 and switch S1.
- **Buffering:** C1 (1000 µF) absorbs inrush current and LED load steps.
- **Data line:** The ESP8266 runs at 3.3 V, the LEDs expect 5 V logic. The 74HCT125 (U2) lifts the signal from `RX`/GPIO3 to 5 V (HCT input: V<sub>IH</sub> ≥ 2.0 V). Only gate 1 is used; `1OE` is tied to GND, so the output is always enabled.
- **R1 (220 Ω)** in series with the data line damps reflections and protects the first LED input.

## Bill of materials

| Ref. | Part | Value / type | Status |
| --- | --- | --- | --- |
| J1 | Power supply | Mean Well GST60A05-P1J, 5 V / 6 A | ✅ available |
| F1 | Fuse | 10 A slow-blow | ❌ missing |
| S1 | Switch | On/off | ✅ available |
| C1 | Electrolytic capacitor | 1000 µF, ≥ 6.3 V | ✅ available |
| U1 | Microcontroller | Wemos D1 mini (ESP8266) | ✅ available |
| U2 | Level shifter | 74HCT125 | ✅ available |
| R1 | Resistor | 220 Ω | ✅ available |
| LED1…n | LED strip | addressable, 5 V | ✅ available |
| U3 | Real-time clock | DS3231 RTC module | ❌ missing (not yet in the schematic) |

## Wiring

| From | To | Net |
| --- | --- | --- |
| J1 + | F1 → S1 → +5 V rail | +5 V |
| +5 V | U1 `5V`, U2 pin 14 (VCC), LED `+5V`, C1 + | +5 V |
| J1 − | U1 `G`, U2 pin 7 (GND), U2 pin 1 (`1OE`), LED `GND`, C1 − | GND |
| U1 `RX` (GPIO3) | U2 pin 2 (`1A`) | Data 3.3 V |
| U2 pin 3 (`1Y`) | R1 → LED `DIN` | Data 5 V |

The data pin matches the firmware: `LED_PIN 3` in `include/config.h` of OpenWordClock-Software.

## Open points

- [ ] Get **fuse F1** and place it directly after J1.
- [ ] Add the **DS3231 RTC**. The firmware starts I²C on the ESP8266 with `Wire.begin(D4, D3)`, so SDA → `D4` (GPIO2) and SCL → `D3` (GPIO0), supply 3.3 V. Both pins are boot-strapping pins; the module's pull-ups keep them high at boot, which is what the ESP8266 needs.
- [ ] Tie off the **unused 74HCT125 gates** (do not leave CMOS inputs floating): `2OE`, `3OE`, `4OE` (pins 4, 10, 13) to +5 V, `2A`, `3A`, `4A` (pins 5, 9, 12) to GND.
- [ ] Add a **100 nF ceramic capacitor** directly between U2 pin 14 and pin 7.

## Notes

- `RX`/GPIO3 is also the ESP8266's UART receive pin, so serial input over USB is not usable; flashing still works. GPIO3 is the DMA pin of the NeoPixelBus library and gives clean timing.
- The D1 mini is powered directly from the 5 V rail via its `5V` pin. When flashing over USB, switch the supply off (S1) so USB and PSU 5 V do not back-feed each other.
- With many LEDs, feed 5 V into the far end of the strip as well (power injection) to limit voltage drop.
