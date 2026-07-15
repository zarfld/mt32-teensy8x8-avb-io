# Interposer Pin Mapping

## Purpose

This document is a **first-draft** pin mapping for the custom passive interposer that connects the JOYNED MT32 30-pin FFC system connector to the Teensy8x8AudioBoard expansion headers.

> ⚠️ **All entries in this table are working assumptions. None have been verified against the physical boards or official datasheets. Do not power up based on this table alone. Verify every row against the MT32 EVK schematic and the traced Teensy8x8AudioBoard expansion header before making any connections.**

The CSV version of this table lives in `hardware/interposer/pinmap/mt32-to-teensy8x8-pinmap.csv`.

---

## Pin Mapping Table

> ⚠️ Each row separates four concepts:
>
> - **Logical signal name**: what the signal does.
> - **Teensy pin equivalent**: the MCU I/O number associated with this signal.
> - **Physical board connection point**: the actual connector pad on the physical PCB — **TBD for all signal rows; trace on actual board before connecting**.
> - **Verification status**: whether this has been confirmed against hardware.
>
> Do not assume any audio/clock/control signal is present on the EXPAND header unless confirmed by tracing the net on the actual board.

| MT32 FFC Pin | MT32 Signal | Direction | Logical Signal Name | Teensy pin equivalent | Physical board connection point | Verification status | Notes |
|---|---|---|---|---|---|---|---|
| 6 | ADC_RST_N_OUT | MT32 → Board | Codec reset / GPIO | GPIO22 equivalent | Physical board connection point TBD — trace on actual board before connecting | **UNVERIFIED** | Active-low. ⚠️ Verify polarity and drive level before connect. |
| 9 | SDIOD_IN | Board → MT32 | TDM DO (codec output) | Pin 8 equivalent | Physical board connection point TBD — trace on actual board before connecting | **UNVERIFIED** | Capture audio from board. |
| 13 | SDIOB_OUT | MT32 → Board | TDM DI (codec input) | Pin 7 equivalent | Physical board connection point TBD — trace on actual board before connecting | **UNVERIFIED** | Playback audio to board. |
| 17 | SCLK_OUT | MT32 → Board | BCLK | Pin 21 equivalent | Physical board connection point TBD — trace on actual board before connecting | **UNVERIFIED** | ⚠️ Board may invert BCLK — see open questions. |
| 19 | LRCK_OUT | MT32 → Board | LRCK / SYNC | Pin 20 equivalent | Physical board connection point TBD — trace on actual board before connecting | **UNVERIFIED** | Frame sync. Verify polarity. |
| 21 | MCLK_OUT | MT32 → Board | MCLK | Pin 23 equivalent | Physical board connection point TBD — trace on actual board before connecting | **UNVERIFIED** | 12.288 or 24.576 MHz — verify frequency. |
| 23 | SDA | Bidirectional | I2C SDA | Teensy Wire SDA | Physical board connection point TBD — trace on actual board before connecting | **UNVERIFIED** | Shared bus with PCA9546. Verify pull-up location. |
| 24 | SCL | MT32 → Board | I2C SCL | Teensy Wire SCL | Physical board connection point TBD — trace on actual board before connecting | **UNVERIFIED** | Verify pull-up location. |
| TBD | 5 V | Power | 5 V power input | Board 5 V rail | Power header — verify current strategy before connecting | **UNVERIFIED** | ⚠️ See power safety rules below. Do not connect by default. |
| TBD | 3.3 V | Power | 3.3 V power | Board 3.3 V rail | Power header — verify current strategy before connecting | **UNVERIFIED** | ⚠️ See power safety rules below. Do not connect by default. |
| TBD | GND | Power | GND | GND | Power header | **UNVERIFIED** | Multiple GND pins — connect only after checking for shorts and confirming ground strategy. |

---

## Power Strategy Notes

> ⚠️ **Power pin assignments and power strategy must be verified before any hardware is connected. Incorrect power connections can permanently damage both the MT32 and the Teensy8x8AudioBoard.**

### Safety Rule: Do Not Directly Connect MT32 Power Rails by Default

**The initial interposer revision must not directly connect MT32 5 V or 3.3 V rails to the Teensy8x8AudioBoard by default.** Use jumpers, solder bridges, or explicit DNP (Do Not Populate) links for optional power connection only — and only after explicit verification.

Initial safe bring-up procedure:

- Power the Teensy8x8AudioBoard from a **separate current-limited bench 5 V supply** (start at ≤ 200 mA limit).
- Connect MT32 and Teensy8x8 **grounds only after checking for shorts** and confirming the ground strategy.
- Leave MT32 5 V and 3.3 V **unconnected** unless explicitly enabled by a verified jumper/solder bridge strategy.
- **No back-feeding of regulators** — if both boards have their own 3.3 V regulators, connecting the 3.3 V rails together will cause them to fight. Verify isolation before any connection.
- All power pin entries remain marked **UNVERIFIED** until explicitly confirmed.

Key questions still open:

- Can the MT32 FFC 5 V supply the Teensy8x8AudioBoard's total current draw (codec bank + wing boards)?
- Are the MT32 3.3 V and the board's 3.3 V regulator output isolated, or will they fight?
- Should the board be powered from a separate external supply and the MT32 FFC 5 V/3.3 V left open?

---

## Reset Behavior Notes

> ⚠️ **Codec reset pin behavior requires verification:**
>
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
