# Wortuhr

Selbstgebaute Wortuhr auf Basis eines **Wemos D1 mini (ESP8266)** und eines adressierbaren 5-V-LED-Streifens. Die Uhrzeit kommt per WLAN (NTP), später zusätzlich aus einem DS3231-RTC-Modul, und wird als Text angezeigt („ES IST VIERTEL NACH DREI“).

Als Firmware wird [ESPWortuhr/Wortuhr](https://github.com/ESPWortuhr/Wortuhr) verwendet (PlatformIO, Env `ESP8266`). Die Datenleitung liegt dort standardmäßig auf `RX`/GPIO3 – passend zu diesem Schaltplan.

## Schaltplan

![Schaltplan](hardware/schaltplan.svg)

Ursprüngliche Handskizze: [`hardware/schaltplan_skizze.png`](hardware/schaltplan_skizze.png)

### Funktionsprinzip

- **Versorgung:** Das 5-V-Netzteil (J1) speist über Sicherung F1 und Schalter S1 die gemeinsame +5-V-Schiene für ESP, Level Shifter und LEDs.
- **Pufferung:** C1 (1000 µF) fängt Einschaltströme und Lastsprünge der LEDs ab.
- **Datenleitung:** Der ESP8266 arbeitet mit 3,3 V, die LEDs erwarten 5-V-Pegel. Der 74HCT125 (U2) hebt das Signal von `RX`/GPIO3 auf 5 V an (HCT-Eingang: V<sub>IH</sub> ≥ 2,0 V). Nur Gatter 1 wird genutzt, `1OE` liegt fest auf GND (Ausgang immer aktiv).
- **R1 (220 Ω)** in der Datenleitung dämpft Reflexionen und schützt den ersten LED-Eingang.

## Stückliste

Auch als CSV: [`hardware/stueckliste.csv`](hardware/stueckliste.csv)

| Ref. | Bauteil | Wert / Typ | Status |
|------|---------|-----------|--------|
| J1 | Netzteil | Mean Well GST60A05-P1J, 5 V / 6 A | ✅ vorhanden |
| F1 | Sicherung | 10 A träge | ❌ fehlt |
| S1 | Schalter | Ein/Aus | ✅ vorhanden |
| C1 | Elektrolytkondensator | 1000 µF, ≥ 6,3 V | ✅ vorhanden |
| U1 | Mikrocontroller | Wemos D1 mini (ESP8266) | ✅ vorhanden |
| U2 | Level Shifter | 74HCT125 | ✅ vorhanden |
| R1 | Widerstand | 220 Ω | ✅ vorhanden |
| LED1…n | LED-Streifen | adressierbar, 5 V (z. B. WS2812B) | ✅ vorhanden |
| U3 | Echtzeituhr | DS3231 RTC-Modul | ❌ fehlt (noch nicht im Schaltplan) |

## Verdrahtung

| Von | Nach | Netz |
|-----|------|------|
| J1 + | F1 → S1 → +5-V-Schiene | +5 V |
| +5 V | U1 `5V`, U2 Pin 14 (VCC), LED `+5V`, C1 + | +5 V |
| J1 − | U1 `G`, U2 Pin 7 (GND), U2 Pin 1 (`1OE`), LED `GND`, C1 − | GND |
| U1 `RX` (GPIO3) | U2 Pin 2 (`1A`) | Daten 3,3 V |
| U2 Pin 3 (`1Y`) | R1 → LED `DIN` | Daten 5 V |

## Offene Punkte

- [ ] **Sicherung F1** beschaffen und direkt hinter J1 einsetzen.
- [ ] **DS3231 RTC** ergänzen: I²C an `D2`/GPIO4 (SDA) und `D1`/GPIO5 (SCL), Versorgung 3,3 V.
- [ ] **Unbenutzte Gatter des 74HCT125 beschalten** (CMOS-Eingänge nicht offen lassen): `2OE`, `3OE`, `4OE` (Pins 4, 10, 13) auf +5 V, `2A`, `3A`, `4A` (Pins 5, 9, 12) auf GND.
- [ ] **100-nF-Keramikkondensator** direkt zwischen U2 Pin 14 und Pin 7.

## Hinweise

- `RX`/GPIO3 ist gleichzeitig der UART-Empfang des ESP8266 – serielle Eingaben über USB gehen damit nicht, Flashen funktioniert weiterhin. Vorteil: GPIO3 ist der DMA-Pin der NeoPixelBus-Library und liefert sauberes Timing.
- Der D1 mini hängt über `5V` direkt an der Versorgung. Beim Flashen per USB möglichst das Netzteil ausschalten (S1), damit sich USB- und Netzteil-5 V nicht gegenseitig speisen.
- Bei vielen LEDs die 5 V zusätzlich am Ende des Streifens einspeisen (Power Injection), um Spannungsabfall zu vermeiden.

## Projektstruktur

```
wortuhr/
├── README.md
└── hardware/
    ├── schaltplan.svg          # Schaltplan (Vektor)
    ├── schaltplan.png          # Schaltplan (Raster)
    ├── schaltplan_skizze.png   # ursprüngliche Handskizze
    └── stueckliste.csv         # Stückliste
```
