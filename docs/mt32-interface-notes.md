# MT32 Interface Notes

## Overview

The JOYNED MT32 is a Milan/AVB endpoint SoM that exposes its audio and control signals through a 30-pin FFC system connector.

> ⚠️ **All pin assignments below are working assumptions based on available documentation and should be verified against the MT32 datasheet and EVK schematics before any hardware is connected.**

Reference: `external/joyned_mt32-evk` submodule (see [`docs/submodules.md`](submodules.md)).

---

## FFC Connector Overview

The MT32 uses a 30-pin FFC connector for the system interface. Key signal groups:

- TDM/I2S audio data (SDIOB_OUT for playback, SDIOD_IN for capture)
- Clock outputs (MCLK_OUT, SCLK_OUT/BCLK, LRCK_OUT/SYNC)
- I2C control bus (SDA, SCL)
- GPIO / codec reset (ADC_RST_N_OUT)
- Power rails (5 V, 3.3 V, GND)

---

## FFC Pin Assignments (Working Assumptions)

| FFC Pin | Signal Name | Direction (MT32 perspective) | Notes |
|---|---|---|---|
| 6 | ADC_RST_N_OUT | Output | Active-low codec reset — verify polarity and drive strength |
| 9 | SDIOD_IN | Input | Capture TDM data from codec DO |
| 13 | SDIOB_OUT | Output | Playback TDM data to codec DI |
| 17 | SCLK_OUT | Output | BCLK — bit clock |
| 19 | LRCK_OUT | Output | LRCK / frame sync |
| 21 | MCLK_OUT | Output | Master clock (12.288 MHz or 24.576 MHz typical) |
| 23 | SDA | Bidirectional | I2C data |
| 24 | SCL | Output | I2C clock |
| — | 5 V | Power | Verify current capability |
| — | 3.3 V | Power | Verify current capability |
| — | GND | Power | Multiple pins |

> All assignments require cross-checking against official MT32 EVK documentation. Do not power up based on this table alone.

---

## Power Safety Rule

> ⚠️ **Do not directly connect MT32 FFC power rails (5 V or 3.3 V) to the Teensy8x8AudioBoard by default.**
>
> The initial interposer must use jumpers, solder bridges, or DNP (Do Not Populate) links for power connection — enabled only after explicit verification of current budgets and regulator isolation.
>
> Initial safe bring-up:
>
> - Power the Teensy8x8AudioBoard from a **separate current-limited bench 5 V supply**.
> - Connect MT32 and Teensy8x8 **grounds only after checking for shorts** and confirming the ground strategy.
> - Leave MT32 5 V and 3.3 V **unconnected** until the power strategy is verified.
> - **No back-feeding of regulators** — if both boards provide 3.3 V, do not connect these rails until confirmed safe.

---

## Known Unknowns / Verification Tasks

- Exact mapping of all 30 FFC pins (not just audio/clock/I2C subset).
- Whether SDIOB/SDIOD are multi-slot TDM or simple stereo I2S.
- Number of TDM slots per frame at target sample rates (48 kHz, 96 kHz).
- BCLK and LRCK polarity and phase assumptions.
- Whether MCLK_OUT is always active or requires configuration.
- Current sourcing limits on the 5 V and 3.3 V FFC power pins.
- I2C bus address conflicts with the PCA9546 mux and codec addresses.
- GPIO voltage levels (3.3 V or 1.8 V I/O).

---

## Related Documents

- [`docs/interposer-pinmap.md`](interposer-pinmap.md) — Full interposer mapping table.
- [`docs/tdm-format-open-questions.md`](tdm-format-open-questions.md) — TDM timing open questions.
