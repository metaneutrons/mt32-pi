# Conventions

## Naming

- Classes: `C` prefix, PascalCase — `CMT32Pi`, `CSynthBase`, `CRingBuffer`
- Enums: `T` prefix, PascalCase — `TSynth`, `TLCDType`, `TAudioOutputDevice`
- Member variables: `m_` prefix, then type hint — `m_pSynth` (pointer), `m_nSampleRate` (numeric), `m_bRunning` (bool)
- Static members: `s_` prefix — `s_pThis`
- Constants: `PascalCase` or `UPPER_SNAKE` — `LCDUpdatePeriodMillis`, `MIDIRxBufferSize`
- Files: lowercase, matching class name without prefix — `mt32pi.cpp`, `synthbase.h`

## File Organization

- Headers in `include/`, sources in `src/`, mirrored subdirectory structure
- One class per file pair (header + source)
- Subdirectories by subsystem: `synth/`, `lcd/`, `lcd/drivers/`, `net/`, `control/`

## Header Style

- Include guards: `#ifndef _filename_h` / `#define _filename_h`
- GPL v3 license header on every file
- Circle headers first, then stdlib, then project headers

## Code Patterns

- **Singleton**: `CMT32Pi` uses static `s_pThis` pointer
- **Abstract base + concrete**: `CSynthBase` → `CMT32Synth`, `CSoundFontSynth`; `CLCD` → `CHD44780`, `CSSD1306`
- **X-macros**: `config.def` defines all config options once, expanded differently in `config.h` (struct members) and `config.cpp` (parser)
- **Ring buffer**: Lock-free `CRingBuffer<T, Size>` for MIDI data between cores
- **Event queue**: Decoupled input → processing via `TEventQueue`
- **Spinlocks**: `CSpinLock` for synth render/MIDI thread safety (multi-core)
- **Callbacks**: Static handler functions with `s_pThis` dispatch (USB MIDI, IRQ)
- **volatile**: Used on members accessed across cores (`m_bRunning`, `m_pConfig`, `m_pUSBMassStorageDevice`)

## Config System

`config.def` uses a macro table:
```
CFG(<ini_name>, <type>, <member_name>, <default_value>, <extra_args>...)
```
Sections: `system`, `midi`, `audio`, `control`, `mt32emu`, `fluidsynth`, `lcd`, `network`.

## Logging

Circle's `LOGMODULE()` macro per file, then `LOGNOTE()`, `LOGWARN()`, `LOGERR()`, `LOGDBG()`.

## Editor Config

- Tabs for indentation (tab width unspecified in `.editorconfig`)
- UTF-8, LF line endings
- Trim trailing whitespace, insert final newline
- Python scripts: black formatter, isort, flake8
