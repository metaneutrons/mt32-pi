# Tech Stack

## Language

C++17 (baremetal, no standard OS runtime). Some C for external libraries (inih).

## Build System

GNU Make with two-stage build:
1. `Makefile` — builds dependencies (circle-stdlib, mt32emu, fluidsynth) via Make + CMake
2. `Kernel.mk` — builds the mt32-pi kernel itself, included from Circle's `Rules.mk`

Configuration in `Config.mk`: board selection, toolchain prefix, paths, flags.

Version string auto-generated from `git describe --tags`.

### Build Targets

| Board     | Arch    | Bits | CPU           | Kernel file     |
|-----------|---------|------|---------------|-----------------|
| pi2       | ARMv7   | 32   | Cortex-A7     | kernel7         |
| pi3       | ARMv7   | 32   | Cortex-A53    | kernel8-32      |
| pi3-64    | AArch64 | 64   | Cortex-A53    | kernel8         |
| pi4       | ARMv7   | 32   | Cortex-A72    | kernel7l        |
| pi4-64    | AArch64 | 64   | Cortex-A72    | kernel8-rpi4    |

Default: `pi3-64`

### Toolchains

- `arm-none-eabi-` (32-bit targets)
- `aarch64-none-elf-` (64-bit targets)
- Version: ARM GNU Toolchain 11.3.rel1

### Compiler Flags

- `-Werror -Wextra` on kernel code
- `-Ofast` for mt32emu and fluidsynth
- `-ffunction-sections -fdata-sections` with `--gc-sections` for size optimization
- Optional gzip kernel compression

## External Dependencies (Git Submodules)

| Dependency     | Purpose                              | Path                      |
|----------------|--------------------------------------|---------------------------|
| circle-stdlib  | Baremetal C/C++ framework + newlib   | external/circle-stdlib    |
| Circle         | RPi hardware abstraction (nested)    | via circle-stdlib         |
| Munt (mt32emu) | Roland MT-32/CM-32L emulation        | external/munt             |
| FluidSynth     | SoundFont-based MIDI synthesis       | external/fluidsynth       |
| inih           | INI file parser                      | external/inih             |

Circle provides: GPIO, USB, I²C, SPI, networking, audio (PWM/I²S/HDMI), FAT filesystem, Wi-Fi (via hostap/wpa_supplicant), scheduler, multi-core support.

Patches applied to dependencies at build time (in `patches/`):
- `circle-45-minimal-usb-drivers.patch` — strip unused USB drivers
- `circle-45-cp210x-remove-partnum-check.patch` — broader USB serial support
- `circle-45-gzip-kernel.patch` — gzip kernel compression
- `fluidsynth-2.3.1-circle.patch` — port FluidSynth to Circle baremetal

## CI

GitHub Actions (`ci.yml`): lint (black, isort, flake8, shellcheck on Python/shell scripts) + matrix build across all board targets.

## Scripts

- `mt32pi_installer.sh` — interactive installer for Linux/MiSTer
- `mt32pi_updater.py` — Python-based firmware updater (connects via network)

## SD Card Layout

```
SD:/
├── config.txt          # RPi boot config
├── cmdline.txt         # Kernel command line
├── mt32-pi.cfg         # Application config (INI format)
├── wpa_supplicant.conf # Wi-Fi credentials
├── roms/               # MT-32/CM-32L ROM files (user-provided)
├── soundfonts/         # .sf2 SoundFont files
└── kernel*.img         # The baremetal kernel binary
```
