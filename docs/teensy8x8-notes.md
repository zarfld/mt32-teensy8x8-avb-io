# Teensy8x8AudioBoard Notes

## Overview

The Teensy8x8AudioBoard (by palmerr23) is an open-hardware board designed around a Teensy 4.x microcontroller. It provides:

- Four TLV320AIC3104 audio codec ICs (16 ADC + 16 DAC channels total, 8×8 active in first milestone).
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

> ⚠️ The columns below separate four concepts that must not be conflated:
>
> - **Logical signal**: what the signal does.
> - **Teensy pin equivalent**: the MCU I/O number associated with this signal in the firmware.
> - **Physical board connection point**: the actual pad or connector pin on the physical PCB — **TBD for each signal; trace on actual board before connecting**.
> - **Verification status**: whether this has been confirmed on the real board.
>
> Do not assume any signal is present on the EXPAND header unless confirmed by tracing the net on the actual board.

| Logical Signal | Teensy pin equivalent | Physical board connection point | Verification status | Notes |
|---|---|---|---|---|
| TDM Data In (DI) | 7 | Physical board connection point TBD — trace on actual board before connecting | **UNVERIFIED** | To codec DI |
| TDM Data Out (DO) | 8 | Physical board connection point TBD — trace on actual board before connecting | **UNVERIFIED** | From codec DO |
| LRCK / SYNC | 20 | Physical board connection point TBD — trace on actual board before connecting | **UNVERIFIED** | Frame sync |
| BCLK | 21 | Physical board connection point TBD — trace on actual board before connecting | **UNVERIFIED** | Bit clock — **may be inverted on board, see open questions** |
| MCLK | 23 | Physical board connection point TBD — trace on actual board before connecting | **UNVERIFIED** | Master clock |
| I2C SDA | Teensy Wire SDA | Physical board connection point TBD — trace on actual board before connecting | **UNVERIFIED** | Shared with PCA9546 mux |
| I2C SCL | Teensy Wire SCL | Physical board connection point TBD — trace on actual board before connecting | **UNVERIFIED** | Shared with PCA9546 mux |
| Codec reset / GPIO | 22 | Physical board connection point TBD — trace on actual board before connecting | **UNVERIFIED** | Active-low reset |
| 5 V | USB/power in | Power header | **UNVERIFIED** | |
| 3.3 V | Teensy 3.3 V out | Power header | **UNVERIFIED** | |
| GND | GND | Power header | **UNVERIFIED** | |

> All Teensy pin equivalents are for the Teensy 4.x footprint. These assignments are working assumptions only.
> **Do not assume DI, DO, MCLK, BCLK, LRCK, or reset are directly available on the EXPAND header unless verified by tracing the actual board.**
> Trace each net from the Teensy MCU footprint through the PCB to the actual connector pad before building the interposer.

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
