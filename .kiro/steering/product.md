# Product

## Overview

mt32-pi is a baremetal MIDI synthesizer for Raspberry Pi. It turns a Pi into a dedicated hardware sound module — primarily emulating the Roland MT-32/CM-32L, with additional General MIDI/GS/XG support via SoundFonts.

No OS runs on the Pi; the firmware boots directly into the synthesizer application using the Circle baremetal framework. This yields very low audio latency and cold-boot-to-playing in seconds.

## Author

Dale Whinham (dwhinham). Sole developer with 515 commits. Copyright 2020–2023.

## Status

**Effectively discontinued.** The README states no further releases are likely due to sustained community abuse. The last commit (075b528) was Feb 4, 2025 — a README update. The last functional code changes date to late 2022/early 2023.

The project is stable and fully usable but considered "early stages" by its author. Planned features (UI menu system, advanced MIDI routing) were never completed.

## License

GNU General Public License v3.0. The logo has separate, more restrictive terms (no commercial use without permission).

## Target Users

- Vintage PC enthusiasts needing MT-32 sound for DOS games
- MiSTer FPGA users wanting external MIDI synthesis
- PC-98 and Sharp X68000 retro computing setups
- Anyone wanting a dedicated hardware MIDI sound module

## Key Features

- Roland MT-32/CM-32L emulation via Munt
- SoundFont synthesis via FluidSynth (General MIDI, Roland GS, Yamaha XG)
- Bundled GeneralUser GS SoundFont
- MIDI input: USB, GPIO, serial port, network (RTP-MIDI / AppleMIDI, raw UDP)
- Audio output: PWM (headphone jack), I²S Hi-Fi DAC, HDMI
- LCD/OLED display support (HD44780, SSD1306, SH1106)
- Physical controls (buttons, rotary encoder)
- MiSTer FPGA integration via user port
- Embedded FTP server for remote file management
- Wi-Fi networking support
- INI-based configuration file

## Supported Hardware

- Raspberry Pi Zero 2 W, Pi 3 (A+/B/B+), Pi 4 B, CM4
- Pi 2 works with quality concessions; Pi Zero (original) and Pi 1 unsupported
- Build targets: pi2, pi3, pi3-64, pi4, pi4-64
