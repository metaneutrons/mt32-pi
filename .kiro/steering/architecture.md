# Architecture

## Runtime Model

Baremetal — no OS. The Circle framework provides hardware abstraction (GPIO, USB, networking, audio, SPI, I²C). Multi-core execution on the Pi's ARM CPU:

- **Core 0 (MainTask)**: MIDI input polling (USB, serial, network), event processing, control surface updates, MiSTer communication
- **Core 1 (UITask)**: LCD/OLED display updates at ~60fps
- **Core 2 (AudioTask)**: Audio rendering — calls the active synth engine's `Render()` and feeds samples to the sound device

## Central Class: `CMT32Pi`

`src/mt32pi.cpp` (~36K lines) is the application core. Inherits from `CMultiCoreSupport`, `CPower`, `CMIDIParser`, `CAppleMIDIHandler`, `CUDPMIDIHandler`. Owns all subsystems and orchestrates initialization, the main loop, and inter-core communication.

Singleton pattern via `static CMT32Pi* s_pThis`.

## Source Layout

```
src/
├── main.cpp              # Entry point, creates kernel
├── kernel.cpp            # CKernel — Circle boot, creates CMT32Pi
├── mt32pi.cpp            # Main application logic (central orchestrator)
├── config.cpp            # INI config parser (driven by config.def macros)
├── midiparser.cpp        # Raw MIDI byte stream → messages
├── midimonitor.cpp       # Per-channel activity/level tracking
├── rommanager.cpp        # MT-32 ROM scanning and identification
├── soundfontmanager.cpp  # SoundFont file scanning
├── pisound.cpp           # Pisound HAT support
├── power.cpp             # Power management, throttle/undervoltage detection
├── zoneallocator.cpp     # Custom memory allocator
├── synth/
│   ├── mt32synth.cpp     # Munt-based MT-32 emulation
│   └── soundfontsynth.cpp # FluidSynth-based GM/GS/XG synthesis
├── lcd/
│   ├── ui.cpp            # User interface rendering (channel levels, messages)
│   └── drivers/          # HD44780 (4-bit, I²C), SSD1306, SH1106
├── net/
│   ├── applemidi.cpp     # RTP-MIDI / AppleMIDI protocol
│   ├── udpmidi.cpp       # Raw UDP MIDI receiver
│   ├── ftpdaemon.cpp     # FTP server daemon
│   └── ftpworker.cpp     # FTP session handler
└── control/
    ├── control.cpp       # Control surface abstraction
    ├── mister.cpp        # MiSTer FPGA user port protocol
    ├── rotaryencoder.cpp # Rotary encoder input
    ├── simplebuttons.cpp # Button input
    └── simpleencoder.cpp # Simplified encoder input
```

## Data Flow

```
MIDI Source (USB/GPIO/Serial/Network)
  → CMT32Pi::UpdateMIDI() / IRQ handler
  → CRingBuffer<u8, 2048>
  → CMIDIParser (byte stream → short messages / SysEx)
  → CSynthBase* m_pCurrentSynth (MT32Synth or SoundFontSynth)
  → Render() called from AudioTask on Core 2
  → CSoundBaseDevice (PWM / I²S / HDMI)
  → Audio output
```

## Key Abstractions

- **CSynthBase** (abstract): Common interface for synth engines — `HandleMIDIShortMessage()`, `HandleMIDISysExMessage()`, `Render()`, `AllSoundOff()`, `SetMasterVolume()`, `UpdateLCD()`
- **CLCD** (abstract): Display interface — character and graphical modes, with concrete drivers for HD44780 and SSD1306/SH1106
- **CControl**: Control surface abstraction for buttons and encoders
- **CMIDIParser**: Stateful MIDI byte parser with callbacks for short messages and SysEx
- **CConfig**: Macro-driven config system — `config.def` declares all options with types and defaults, expanded via X-macros in `config.h`/`config.cpp`

## Event System

`TEventQueue` (ring buffer) decouples input from processing. Button presses, MiSTer commands, and custom SysEx generate events consumed by `ProcessEventQueue()` on the main core. Events include synth switching, ROM set changes, SoundFont selection, and volume control.

## Custom SysEx

mt32-pi defines its own SysEx commands (manufacturer ID-based) for remote control: reboot, switch MT-32 ROM set, switch SoundFont, switch synth engine, toggle reversed stereo.
