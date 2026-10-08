# Firmware

The software source for this project is the [OpenWordClock-Software repository](https://github.com/openclock/OpenWordClock-Software), a fork of [ESPWortuhr/Wortuhr](https://github.com/ESPWortuhr/Wortuhr). It is built with PlatformIO; the ESP8266 environment is `nodemcuv3`.

## Settings relevant to this hardware

From `include/config.h` in my local checkout:

| Setting | Value | Meaning |
| --- | --- | --- |
| `LED_PIN` | `3` | LED data on `RX`/GPIO3, matches the [schematic](../electronics/schematic/) |
| `Wire.begin(D4, D3)` (in `src/Wortuhr.cpp`) | SDA = D4, SCL = D3 | I²C pins for the RTC on the ESP8266 |
| `RTC_Type` | `RTC_DS3231` | External RTC type |
| `DEFAULT_LAYOUT` | `Ger10x11` | German front, 10 rows × 11 LEDs + 4 minute LEDs |
| `MINUTE_LED4x` | defined | 4 separate minute LEDs |
| `MEANDER_ROWS` | `true` | LED strip runs in a serpentine pattern |

Most of these can also be changed later in the clock's web interface.

TODO: Record the exact upstream revision flashed to the clock and any further project-specific changes. The upstream repository's README describes the software as BSD-3 licensed; preserve its applicable copyright and license notices if source files are copied here.
