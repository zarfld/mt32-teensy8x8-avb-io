# Codec Initialisation Notes

## TLV320AIC3104 Overview

The Teensy8x8AudioBoard uses two Texas Instruments TLV320AIC3104 stereo audio codecs, each providing 4 ADC inputs and 4 DAC outputs (8 channels total per codec IC, 16 channels total for the board — though the board is configured for 8×8 in the standard bring-up).

The codecs are controlled over I2C. Each codec is accessed by selecting the appropriate channel on the PCA9546A I2C mux before writing register commands.

---

## PCA9546A I2C Mux

The PCA9546A is a 4-channel I2C mux that sits between the I2C master (Teensy or MT32) and the individual codec ICs.

- **I2C address:** 0x70–0x77 depending on A0–A2 pin states. Verify on the Teensy8x8AudioBoard silkscreen or schematic.
- To select a channel, write a single byte to the mux address: `0x01` for channel 0, `0x02` for channel 1, etc. Write `0x00` to deselect all channels.
- Only one channel should be selected at a time to avoid I2C address collisions between the two codec ICs.

---

## TLV320AIC3104 I2C Address

The TLV320AIC3104 has a configurable I2C address based on the CS pin:

- CS tied low: 0x18
- CS tied high: 0x19

Verify which address is used for each codec on the Teensy8x8AudioBoard.

---

## Minimum Initialisation Sequence (Working Notes)

> ⚠️ These are working notes based on the TLV320AIC3104 datasheet and the `control_TLV320AIC3104` library. They have not been verified against the Teensy8x8AudioBoard bring-up. Use the upstream library as the authoritative reference.

1. Assert codec reset (ADC_RST_N low) for ≥ 10 ns.
2. Release reset (ADC_RST_N high).
3. Select PCA9546 mux channel for codec 0.
4. Write TLV320AIC3104 register 0x00 (page select) = 0x00 to select page 0.
5. Write register 0x01 (software reset) = 0x80 to trigger a software reset.
6. Configure audio serial interface mode (TDM or I2S), bit depth, and slot assignments.
7. Configure PLL / MCLK divider for target sample rate.
8. Configure ADC/DAC channel mapping and gain.
9. Power up ADC and DAC channels.
10. Repeat for codec 1 (select mux channel for codec 1, then repeat register writes).

See `external/control_TLV320AIC3104` for the Arduino library implementation of the above.

---

## MT32-Driven Initialisation (Future)

When the MT32 drives the system without a Teensy MCU, codec initialisation must be performed by the MT32 firmware via I2C. The MT32 EVK firmware (see `external/joyned_mt32-evk`) may provide hooks for this. This is a future task and is not yet implemented.

---

## Related Documents

- [`docs/tdm-format-open-questions.md`](../docs/tdm-format-open-questions.md)
- [`docs/teensy8x8-notes.md`](../docs/teensy8x8-notes.md)
- [`docs/bringup-plan.md`](../docs/bringup-plan.md)
