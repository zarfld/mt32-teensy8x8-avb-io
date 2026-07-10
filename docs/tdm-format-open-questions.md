# TDM Format Open Questions

This document tracks unresolved questions about TDM/I2S timing between the MT32 and the TLV320AIC3104 codec bank on the Teensy8x8AudioBoard.

None of these questions have been experimentally verified yet. All answers must be confirmed with an oscilloscope or logic analyser before relying on them.

---

## Open Questions

### 1. TDM Slot Width

- What is the TDM slot width used by the MT32? (16-bit, 24-bit, or 32-bit slots?)
- Does the MT32 use a fixed 256-bit frame (8 × 32-bit slots) at 48 kHz, giving 12.288 MHz BCLK?
- Does the TLV320AIC3104 mode (TDM or I2S) need to match the MT32 slot layout exactly?

### 2. Sample Width

- What PCM sample width does the MT32 use for TDM audio? (16, 24, or 32 bits?)
- Is the sample MSB-first or LSB-first?
- Are unused bits within a slot zero-padded or undefined?

### 3. BCLK Polarity

- Does the MT32 clock data on the rising or falling edge of BCLK?
- Does the TLV320AIC3104 sample data on the rising or falling edge?
- Are these two settings compatible, or does the interposer need to invert BCLK?

### 4. Frame Sync (LRCK) Polarity

- Is LRCK high for left (even slot) or for right (odd slot)?
- Is LRCK a one-cycle pulse (TDM-style frame sync) or a 50% duty cycle (I2S-style)?
- Does the MT32 produce I2S-compatible LRCK or TDM-mode LRCK?

### 5. Data Delay Relative to Frame Sync

- Does the MT32 assert data on the cycle immediately following the frame sync edge, or is there a one-cycle delay (I2S convention)?
- The TLV320AIC3104 TDM mode supports a programmable data delay — which setting is needed?

### 6. BCLK Inversion on the Teensy8x8AudioBoard

- The Teensy8x8AudioBoard reportedly includes a BCLK inversion between the Teensy expansion header and the codec bank.
- When the Teensy Audio library drives the board, this inversion is expected and compensated for in the I2S peripheral settings.
- When the MT32 drives BCLK directly through the interposer, the codec will see an inverted BCLK relative to the MT32 output.
- **Does the MT32's BCLK polarity happen to match what the codec expects (making the inversion a non-issue), or does the interposer need a one-gate BCLK inversion buffer?**
- This must be resolved on the oscilloscope before TDM data is clocked.

### 7. Codec Tri-State on Shared TDM DO

- The four TLV320AIC3104 codecs share a single TDM serial data output line (DO) to the MT32 FFC SDIOD_IN.
- Each codec must tri-state its output when not transmitting its assigned slot(s).
- **Are the codecs correctly configured for TDM slot-based tri-stating out of reset, or must the MT32 (or initialisation firmware) explicitly program each codec's active slot range before driving any data onto the shared line?**
- Failure to tri-state correctly will cause bus contention on the shared DO line.

### 8. TLV320AIC3104 Initialisation via PCA9546 I2C Mux

- The four codecs are accessed through a PCA9546A I2C mux.
- The MT32 must act as I2C master and select the correct mux channel before writing to each codec.
- **What I2C address does the PCA9546A appear on?** (Default: 0x70–0x77 depending on A0–A2 pins — verify on board.)
- **What I2C addresses do the TLV320AIC3104 codecs appear on once the mux channel is selected?** (Typically 0x18 or 0x19 depending on CS pin — verify on board.)
- **Can the MT32 firmware enumerate and initialise the codecs, or does a separate microcontroller (Teensy or other) handle codec initialisation and the MT32 handles only audio streaming?**

---

## Measurement Plan

To answer the above questions:

1. Bring up the Teensy8x8AudioBoard with a Teensy MCU (Stage 1 of bringup plan).
2. Run the Teensy Audio library example and capture MCLK, BCLK, LRCK, and DI/DO on a logic analyser.
3. Decode the TDM frame to verify slot width, sample width, framing, and polarity.
4. Repeat with MT32 as the clock/data source (Stage 3) and compare captured signals.

---

## Related Documents

- [`docs/mt32-interface-notes.md`](mt32-interface-notes.md)
- [`docs/teensy8x8-notes.md`](teensy8x8-notes.md)
- [`docs/interposer-pinmap.md`](interposer-pinmap.md)
- [`docs/bringup-plan.md`](bringup-plan.md)
