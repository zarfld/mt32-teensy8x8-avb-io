# Project Overview

## Summary

`mt32-teensy8x8-avb-io` is a prototype integration project that connects the **JOYNED MT32 Milan Endpoint module** to the **palmerr23 Teensy8x8AudioBoard** over TDM/I2S-style clocked audio signals, via a custom passive interposer board.

The goal is a working 8-input / 8-output Milan/AVB audio interface using off-the-shelf or lightly-customized open hardware as a stepping stone toward a more capable pro-line device.

---

## Key Components

### JOYNED MT32
- Milan-certified AVB endpoint module (SoM form factor).
- Provides clocked TDM/I2S audio output and input over a 30-pin FFC system connector.
- Exposes MCLK, BCLK, LRCK, TDM data I/O, I2C, and GPIOs through the FFC.
- Reference: [joyned_mt32-evk](https://github.com/JOYNED-GmbH/joyned_mt32-evk) (see `external/joyned_mt32-evk`).

### Teensy8x8AudioBoard
- Open hardware board by palmerr23 featuring two TLV320AIC3104 codecs on a PCA9546 I2C mux.
- Designed around Teensy 4.x MCU but exposes audio/clock/I2C on expansion headers.
- Supports wing boards for TRS, XLR, or Combo connectors.
- Reference: [Teensy8x8AudioBoard](https://github.com/palmerr23/Teensy8x8AudioBoard) (see `external/Teensy8x8AudioBoard`).

### Custom Interposer
- Passive (or minimally active) adapter PCB that translates the MT32 FFC connector signals to the Teensy8x8AudioBoard pin headers.
- Routes MCLK, BCLK, LRCK, TDM data, I2C, reset GPIO, and power.
- Design files will live in `hardware/interposer/`.

---

## Project Scope (First Milestone)

| Item | Status |
|---|---|
| MT32 FFC pinout verification | ⬜ Pending |
| Teensy8x8AudioBoard header pinout verification | ⬜ Pending |
| Interposer pin mapping | 🔄 Draft (see `docs/interposer-pinmap.md`) |
| Interposer schematic | ⬜ Not started |
| 8×8 bring-up with Teensy | ⬜ Pending |
| 8×8 bring-up with MT32 | ⬜ Pending |

---

## Out of Scope (for First Milestone)

- 16×16 or higher channel count.
- Pro-line +18/+24 dBu input pads (documented but not built yet).
- Custom MT32 daughterboard.
- 1U rack enclosure.
- Phantom power for microphone inputs.

---

## Related Documents

- [`docs/architecture.md`](architecture.md) — System-level block diagram.
- [`docs/interposer-pinmap.md`](interposer-pinmap.md) — Draft pin mapping table.
- [`docs/bringup-plan.md`](bringup-plan.md) — Staged bring-up checklist.
- [`docs/tdm-format-open-questions.md`](tdm-format-open-questions.md) — Unresolved TDM timing questions.
- [`docs/line-level-and-headroom.md`](line-level-and-headroom.md) — Analog level and headroom notes.
