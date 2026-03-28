# platform 16 PCB

This is the KiCad project for **platform 16** — a compact digital synthesizer built around the Raspberry Pi RP2350 microcontroller. The board hosts 16 potentiometers for real-time parameter control, a high-quality audio output chain, and USB-C for power and an SWD port for development.

## Overview

Platform 16 is a self-contained generative synthesizer platform. The hardware is designed to pair with swappable firmware engines — each one turns the same 16 knobs into a different instrument. A companion [front panel editor](https://github.com/lerouxb/platform16-editor) (web app) generates printable overlays so you can label the knobs for whichever [firmware](https://github.com/lerouxb/plaform16-firmware) is loaded.

## Key Components

| Component | Part | Role |
|---|---|---|
| MCU | **RP2350** (QFN-60) | Dual-core Cortex-M33 — runs synthesis DSP, reads pots, drives I2S audio |
| DAC | **PCM5100A** (TI) | 32-bit stereo I2S DAC — converts digital audio to line-level analog |
| Headphone amp | **PAM8908JER** | Capless stereo headphone amplifier — drives headphones without coupling caps |
| Flash | **W25Q128JVS** (Winbond) | 16 MB QSPI flash for firmware and data storage |
| Analog mux | **CD74HC4067SM** | 16-channel analog multiplexer — reads all 16 pots through a single ADC channel |
| Voltage regulator | **NCP1117-3.3** | 3.3 V LDO powered from USB 5 V |
| Audio jacks | **PJ-320D-A** (×3) | 3.5 mm TRS jacks edge-mounted through board notches |
| Connector | **USB-C 16-pin** | Power and data (with proper CC resistors for USB-C detection) |

## Board Details

- **Dimensions:** ~112 mm × 86 mm with rounded corners
- **Mounting:** 4 × M3 mounting holes in a 69 mm × 46 mm rectangle
- **Audio jacks:** Semicircular edge cutouts allow the 3.5 mm jack barrels to protrude through the top edge
- **Passives:** 0402 throughout for a compact layout
- **Potentiometers:** 16 × 10 kΩ through-hole pots (the namesake of the project)
- **Buttons:** 2 × tactile switches for boot mode and reset

## Architecture

```
USB-C (5V)
  │
  ├─► NCP1117 → 3.3V rail
  │
  ▼
RP2350 ──I2S──► PCM5100A (DAC) ──► PAM8908 (amp) ──► 3.5mm jacks
  │
  ├─► CD74HC4067 mux ◄── 16 pots
  ├─► QSPI flash (W25Q128)
  ├─► 2 buttons
  └─► Clock in/out
```

## Schematic

![schematic](images/schematic.png)

## PCB Layout

![traces](images/traces.png)

## 3D Render

![render](images/render.png)

## Related Projects

- **[Firmware](https://github.com/lerouxb/plaform16-firmware)** — C++ (Pico SDK) running on the RP2350. Three swappable generative synth engines:
  - **SDS** — Stochastic Decay Subtractive (PolyBLEP saw + Moog ladder filter, random sequencer)
  - **PMD** — Phase Modulation with Decay (2-operator FM triads, dual LFOs)
  - **TEP** — Two-tone Euclidean Polymeters (dual detunable saws, 3 independent Euclidean rhythms)
- **[Front Panel Editor](https://github.com/lerouxb/platform16-editor)** — React/TypeScript web app for designing printable panel overlays with labelled knobs matching each firmware's control layout

## Tools

- **KiCad 9.0** — schematic and PCB design
- **Fabrication Toolkit** — JLC-compatible production output (BOM, positions, netlist in `production/`)
