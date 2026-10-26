# M5 Editor

A small browser tool for the **Line 6 M5 Stompbox Modeler**. It shows all 24 presets of a DUMP ALL at once, lets you rename and reorder them, shows each preset's knob settings (including the expression-pedal heel/toe range), and sends the result back to the pedal.

**Use it:** open the page in Chrome or Edge (Safari has no Web MIDI). Everything runs in your browser; nothing is uploaded.

## What it does
- Load a DUMP ALL from a `.syx` file, or receive it straight from the M5 over MIDI
- See all 24 presets in two rows of 12, with the effect type and the real effect name
- Rename presets (16 characters) and swap them by dragging
- View the knobs of each preset, with heel and toe markers where the expression control moves a knob
- Save as `.syx`, or send all 24 presets back to the M5

## Backing up and restoring the M5
- **Dump:** M5 MIDI OUT → interface IN. Setup page 2, Model Select to DUMP ALL?, then hold ON/OFF and press Model Select (Line 6 notes the Pilot Guide is wrong about this step).
- **Restore:** interface OUT → M5 MIDI IN, then just send the file. The M5 receives automatically.
- Sending a DUMP ALL replaces all 24 presets. Keep a backup.

## How the format works (short version)
- 244 SysEx messages: 24 presets × 10, plus 4 for global settings
- Data is 7-bit packed (1 high-bits byte + 7 data bytes); each message ends with `(length byte + data) & 0x7F`
- Each preset unpacks to 1,200 bytes and starts with a checksum: the sum of all 32-bit little-endian words from byte 4 onward
- Name at byte 256 (16 chars), effect family at 334, model at 332, knob count at 336, knob values as floats from 340 (4 per knob: flag, current, heel, toe)

## Not yet
- Single-preset dumps (DUMP PRE)
- Real knob names per effect
- Effect names for models not in the factory set (click the triangle to name one; it's remembered in your browser)

## How this was made
Built in one evening with Claude (Anthropic's AI), which did most of the reverse-engineering and coding from dumps I made on my own M5. Tested on real hardware.

MIT licence.
