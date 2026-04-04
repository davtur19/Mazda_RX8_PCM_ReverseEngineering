# RX-8 PCM Reverse Engineering


[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=GA2ATM7VC5LZL&currency_code=USD&source=url)


Here is a dump of all things on the journey to reverse engineer the Mazda RX-8 ECU/PCM and define the tables and modified values for use in ROMraider


Reverse Engineering Tool: Ghidra - https://ghidra-sre.org/
ROMRaider - https://www.romraider.com/

Folders:

Data_Binaries

These are live data binaries, and some ROM files I have taken

Ghidra_Archives

This is where I am stashing my latest ghidra archives. Ghidra (from what I can tell) doesn't have a great way to collaborate, so this is the best I can do
	
Stock_ROMs

Uploaded Stock ROM binary files


More info, and because I can't figure out how to link the repos:

RX8Man - PCM/ECM/ECU reflash tool for use with a Tactrix: https://github.com/Rx8Man/Rx8Man/releases

My current Romraider ECU definitions + RomRaider logger version that supports Mazda CAN: https://github.com/equinox311/RX8Defs

RX8Man's ECU definitions: https://github.com/Rx8Man/RX8Defs

---

## Fork additions (AI-assisted reverse engineering)

This fork adds in-depth firmware analysis for the **N3J1EL ROM** (60E1D400.bin — EU 6-port 231hp MT, 2004-2008), produced using IDA Pro with the SH7055 processor module and Claude AI as analysis assistant. The output has been manually reviewed, but **treat everything as best-effort RE work** — mistakes are possible.

### What was added

**`docs/analysis/`** — Subsystem analysis documents:
- UDS SecurityAccess (SID 0x27) — fixed-key auth, full unlock sequence
- Flash reprogramming (SID 0x34/0x36/0x37) — block transfer protocol, RAM state machine
- EEPROM (S-93C56C) — 256-byte memory map, SPI bit-bang routines, immobilizer data
- OMP (Oil Metering Pump) — dual-table duty control, verified calibration axes
- Throttle (drive-by-wire) — pedal maps, position classification, torque damping
- Cooling / AC — fan control logic, auxiliary fan task
- Control chains — high-level signal flow for fuel, ignition, idle

**`docs/pseudocode/`** — C-like pseudocode for ~2800 functions:
- Complete firmware pseudocode (137KB)
- Idle, VIS/SIV, MAF, catalyst subsystem detail (60KB)

**`docs/maps/`** — Address maps:
- RAM map (~500 variables, confidence-tagged)
- Calibration/ROM map (~200 tables with scaling)

**`ida/`** — IDA Pro database:
- `.i64` database with 2789 named functions, comments, and type annotations

### Disclaimer

> **This documentation was generated with AI assistance (Claude) and reviewed by a human.**
> Function names, addresses, and calibration data were verified against the ROM binary where possible. Some pseudocode, RAM variable purposes, and signal flow descriptions are inferred from disassembly patterns and may contain inaccuracies. Confidence levels are marked where applicable: `[CONFIRMED]`, `[INFERRED]`, `[SPECULATIVE]`.
>
> **Do not blindly trust OMP duty values or fuel tables for tuning.** Always validate on a dyno with proper wideband O2 monitoring. Incorrect OMP calibration can destroy a rotary engine.

### Credits

- **[equinox311](https://github.com/equinox311)** — Original RE work, Ghidra archives, RomRaider definitions, community research. This fork builds on top of their foundational work.
- **[RX8Man](https://github.com/Rx8Man)** — PCM reflash tool and ECU definitions.
- **[rx8club.com community](https://www.rx8club.com/series-i-engine-tuning-forum-63/)** — Years of collective tuning knowledge and hardware documentation.
