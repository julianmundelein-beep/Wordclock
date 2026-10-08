# Wortuhr Build Report

This report documents my own word-clock project: the decisions I made, the parts and files I used, and how the build developed. It is a project record rather than a step-by-step guide for reproducing the clock.

Details that have not yet been provided are marked **TODO**. They should be replaced with notes from the actual build rather than assumptions.

## Project overview

I built a Wortuhr (word clock) and am collecting the related project files and documentation in this repository.

- **Why I started the project:** TODO
- **Build period:** TODO
- **Current state of the clock:** TODO
- **What I wanted to achieve:** TODO

## Design and decisions

TODO: Describe the design of this particular clock and the decisions made during the project. This can include the face layout, enclosure, electronics, and any changes made while developing the build.

## Electronics

Full details: [schematic, BOM and wiring](electronics/schematic/).

- **Controller:** Wemos D1 mini (ESP8266)
- **Lighting:** addressable 5 V LED strip; data from `RX`/GPIO3 through a 74HCT125 level shifter (3.3 V → 5 V) and a 220 Ω series resistor
- **Power supply:** Mean Well GST60A05-P1J (5 V), on/off switch, 1000 µF buffer capacitor on the 5 V rail
- **Other components:** DS3231 RTC module and 10 A slow-blow fuse planned, not yet installed
- **Schematic:** [`electronics/schematic/schaltplan.svg`](electronics/schematic/schaltplan.svg), redrawn from my [hand sketch](electronics/schematic/schaltplan_skizze.png)
- **PCB:** TODO

## 3D-printed parts

TODO: Describe the parts printed for this build, including material, print settings, and any revisions if known.

The project files include [`komplette_Grundplatte_.stl`](3d-print/other-parts/komplette_Grundplatte_.stl), a base-plate STL. TODO: Record how this part was fabricated and whether the file reflects the final version used in the clock.

The repository has separate locations for the enclosure, deckplate, and other printed parts. The source and usage rights for the deckplate have not yet been established; document them before making any licensing or redistribution claims about that file.

## Firmware and software

The software source for this project is the [OpenWordClock-Software repository](https://github.com/openclock/OpenWordClock-Software). Hardware-relevant settings (LED pin, RTC pins, layout) are listed in [`firmware/`](firmware/).

- Exact upstream revision used: TODO
- Project-specific configuration or changes: TODO
- Development environment used for this build: PlatformIO (TODO: confirm)

The upstream repository's README describes the software as BSD-3 licensed. If any upstream source files are copied into this repository, confirm the applicable license text and preserve the required notices.

## Front panel

The project files include [`EdelstahlFrontV6.dxf`](fabrication/stainless-front-panel/EdelstahlFrontV6.dxf), a drawing for the stainless-steel front panel. TODO: Record how it was fabricated and whether this is the final version used in the clock.

## Build notes

TODO: Add a short account of how the project progressed. Include real decisions, problems, revisions, and observations from the build.

## Finished clock

TODO: Describe the result and add a photo from `photos/finished/` when one is available.

## Project files

- `electronics/schematic/` and `electronics/pcb/` for electronics design files
- `3d-print/` for printed parts
- `fabrication/` for fabrication drawings
- `firmware/` for code and software notes
- `photos/build/` and `photos/finished/` for project photos
- `docs/` for supporting notes

**License:** TODO — choose a license for the original project files and record third-party file terms separately where needed.
