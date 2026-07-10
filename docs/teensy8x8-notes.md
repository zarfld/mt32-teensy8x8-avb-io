# Teensy8x8AudioBoard Notes

## Overview

The Teensy8x8AudioBoard (by palmerr23) is an open-hardware board designed around a Teensy 4.x microcontroller. It provides:

- Two TLV320AIC3104 audio codec ICs (8 ADC + 8 DAC channels total).
- A PCA9546A I2C multiplexer for codec control.
- IDC wing connectors for TRS, XLR, or Combo breakout boards.
- Expansion headers exposing audio clock, I2S/TDM data, I2C, and GPIO signals.

Reference: `external/Teensy8x8AudioBoard` and `external/control_TLV320AIC3104` submodules (see [`docs/submodules.md`](submodules.md)).

---

## Board Revision

> ⚠️ The board revision must be identified and the expansion header pinout traced on the physical board before any interposer is built. Different revisions may have different pinouts or jumper configurations.

Pending verification task: see `docs/bringup-plan.md`, Stage 0, item "Verify Teensy8x8 board revision."

---

## Key Expansion Header Signals (Working Assumptions)

| Function | Teensy pin (MCU) | Board header | Notes |
|---|---|---|---|
| TDM Data In (DI) | 7 | Expansion header | To codec DI — verify |
| TDM Data Out (DO) | 8 | Expansion header | From codec DO — verify |
| LRCK / SYNC | 20 | Expansion header | Frame sync — verify |
| BCLK | 21 | Expansion header | Bit clock — **may be inverted on board, see open questions** |
| MCLK | 23 | Expansion header | Master clock — verify |
| I2C SDA | Teensy Wire SDA | Expansion header | Shared with PCA9546 mux |
| I2C SCL | Teensy Wire SCL | Expansion header | Shared with PCA9546 mux |
| Codec reset / GPIO | 22 | Expansion header | Active-low reset — verify |
| 5 V | USB/power in | Power header | |
| 3.3 V | Teensy 3.3 V out | Power header | |
| GND | GND | Power header | |

> All pin numbers are for Teensy 4.x footprint. These assignments are working assumptions — trace the actual board before building the interposer.

---

## BCLK Inversion

The Teensy8x8AudioBoard schematic reportedly inverts BCLK between the Teensy output and the codec bank. This has implications when the MT32 (not the Teensy) drives BCLK:

- The MT32 must produce BCLK with appropriate polarity for the TLV320AIC3104.
- Or the interposer must include an inversion stage (e.g., single CMOS inverter).
- The Teensy Audio library internally accounts for the board inversion; the MT32 does not.

See [`docs/tdm-format-open-questions.md`](tdm-format-open-questions.md) for details.

---

## Wing Connectors

The board has IDC connectors for up to four wing boards. Each wing board provides analog I/O connectivity:

- TRS input wing (balanced line-level input)
- TRS output wing (balanced line-level output)
- XLR input wing
- XLR output wing
- Combo (XLR+TRS) wings
- Phantom-power XLR wings (future)

See [`docs/wing-board-population.md`](wing-board-population.md) for population guide.

---

## Codec: TLV320AIC3104

Texas Instruments TLV320AIC3104 stereo audio codec:

- Up to 96 kHz sample rate.
- Differential or single-ended analog I/O.
- I2C programmable.
- 3.3 V supply.

See [`docs/tdm-format-open-questions.md`](tdm-format-open-questions.md) and `external/control_TLV320AIC3104` for codec initialisation details.

---

## Related Documents

- [`docs/mt32-interface-notes.md`](mt32-interface-notes.md)
- [`docs/interposer-pinmap.md`](interposer-pinmap.md)
- [`docs/wing-board-population.md`](wing-board-population.md)
- [`firmware/codec-init-notes.md`](../firmware/codec-init-notes.md)
