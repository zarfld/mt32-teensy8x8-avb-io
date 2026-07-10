# Interposer Pin Mapping

## Purpose

This document is a **first-draft** pin mapping for the custom passive interposer that connects the JOYNED MT32 30-pin FFC system connector to the Teensy8x8AudioBoard expansion headers.

> ⚠️ **All entries in this table are working assumptions. None have been verified against the physical boards or official datasheets. Do not power up based on this table alone. Verify every row against the MT32 EVK schematic and the traced Teensy8x8AudioBoard expansion header before making any connections.**

The CSV version of this table lives in `hardware/interposer/pinmap/mt32-to-teensy8x8-pinmap.csv`.

---

## Pin Mapping Table

| MT32 FFC Pin | MT32 Signal | Direction | Teensy8x8 Header Signal | Teensy MCU Pin (equiv.) | Notes |
|---|---|---|---|---|---|
| 6 | ADC_RST_N_OUT | MT32 → Board | Codec reset / GPIO | GPIO22 equivalent | Active-low. ⚠️ Verify polarity and drive level before connect. |
| 9 | SDIOD_IN | Board → MT32 | TDM DO (codec output) | Pin 8 equivalent | Capture audio from board. |
| 13 | SDIOB_OUT | MT32 → Board | TDM DI (codec input) | Pin 7 equivalent | Playback audio to board. |
| 17 | SCLK_OUT | MT32 → Board | BCLK | Pin 21 equivalent | ⚠️ Board may invert BCLK — see open questions. |
| 19 | LRCK_OUT | MT32 → Board | LRCK / SYNC | Pin 20 equivalent | Frame sync. Verify polarity. |
| 21 | MCLK_OUT | MT32 → Board | MCLK | Pin 23 equivalent | 12.288 or 24.576 MHz — verify frequency. |
| 23 | SDA | Bidirectional | I2C SDA | Teensy Wire SDA | Shared bus with PCA9546. Verify pull-up location. |
| 24 | SCL | MT32 → Board | I2C SCL | Teensy Wire SCL | Verify pull-up location. |
| TBD | 5 V | Power | 5 V power input | Board 5 V rail | ⚠️ Verify MT32 current capability before connecting. |
| TBD | 3.3 V | Power | 3.3 V power | Board 3.3 V rail | ⚠️ Do not back-feed regulators. Verify isolation strategy. |
| TBD | GND | Power | GND | GND | Multiple GND pins — connect all. |

---

## Power Strategy Notes

> ⚠️ **Power pin assignments and power strategy must be verified before any hardware is connected. Incorrect power connections can permanently damage both the MT32 and the Teensy8x8AudioBoard.**

Key questions:
- Can the MT32 FFC 5 V supply the Teensy8x8AudioBoard's total current draw (codec bank + wing boards)?
- Are the MT32 3.3 V and the board's 3.3 V regulator output isolated, or will they fight?
- Should the board be powered from a separate external supply and the MT32 FFC 5 V/3.3 V left open or use only as a reference?

Recommendation: Use a separate bench power supply for the Teensy8x8AudioBoard during initial bring-up. Connect MT32 FFC power only after verifying current budgets.

---

## Reset Behavior Notes

> ⚠️ **Codec reset pin behavior requires verification:**
> - Is `ADC_RST_N_OUT` truly active-low?
> - What is the output voltage level (3.3 V logic or 1.8 V logic)?
> - Does the MT32 assert reset on startup, or is it driven high by default?
> - Is there a recommended power-on reset sequence (e.g., power → reset assert → delay → release)?

---

## I2C Bus Notes

The Teensy8x8AudioBoard uses a PCA9546A I2C mux. The MT32 I2C master must know the mux channel assignments and the codec register map. Codec initialisation via I2C from the MT32 (without a Teensy MCU) is a future task.

Pull-up resistors for SDA/SCL: verify whether they are on the Teensy8x8AudioBoard or must be on the interposer.

---

## BCLK Inversion

The Teensy8x8AudioBoard reportedly inverts BCLK between the expansion header and the codec bank. When the MT32 drives BCLK directly (bypassing the Teensy), the effective polarity at the codec may be inverted relative to what the MT32 expects. The interposer may need an inversion buffer.

See [`docs/tdm-format-open-questions.md`](tdm-format-open-questions.md).

---

## CSV Source

See `hardware/interposer/pinmap/mt32-to-teensy8x8-pinmap.csv` for the machine-readable version of the above table.

---

## Related Documents

- [`docs/mt32-interface-notes.md`](mt32-interface-notes.md)
- [`docs/teensy8x8-notes.md`](teensy8x8-notes.md)
- [`docs/tdm-format-open-questions.md`](tdm-format-open-questions.md)
