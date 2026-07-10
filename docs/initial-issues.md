# Initial GitHub Issues

This document lists the initial issues that should be created in the GitHub issue tracker for this project.

To create these issues, use the GitHub web UI or the `gh issue create` CLI command with the appropriate template.

---

## Issue List

### Issue 1: Verify exact MT32 FFC pinout against datasheet

**Type:** Research  
**Priority:** High  
**Description:**  
Cross-check the MT32 FFC pin assignments in `docs/interposer-pinmap.md` and `docs/mt32-interface-notes.md` against the official MT32 datasheet and/or EVK schematic (see `external/joyned_mt32-evk`). Document any discrepancies. Mark all verified entries in the pin map.

---

### Issue 2: Verify Teensy8x8 board revision and expansion header pinout

**Type:** Research  
**Priority:** High  
**Description:**  
Identify the exact revision of the Teensy8x8AudioBoard in hand. Locate the expansion header pinout documentation. Cross-check against the working assumptions in `docs/teensy8x8-notes.md` and `docs/interposer-pinmap.md`. Document the board revision and confirm or correct the assumed pin assignments.

---

### Issue 3: Trace DI/DO/MCLK/BCLK/LRCK/reset/I2C nets on the actual board

**Type:** Hardware Task  
**Priority:** High  
**Description:**  
Using a multimeter (continuity mode) and the Teensy8x8AudioBoard schematic, physically trace the DI, DO, MCLK, BCLK, LRCK, codec reset, SDA, and SCL nets from the Teensy/codec expansion header to the codec ICs and IDC wing connectors. Confirm or correct the assumed pinout in `docs/interposer-pinmap.md`.

---

### Issue 4: Confirm safe power strategy before connecting MT32

**Type:** Hardware Task  
**Priority:** High — Safety Critical  
**Description:**  
Determine whether the MT32 FFC power rails (5 V, 3.3 V) can safely supply the Teensy8x8AudioBoard (codec bank, wing boards, I2C mux). Alternatively, define an external bench-power bring-up strategy that avoids back-feeding the MT32 or the board's regulator. Document the agreed power strategy before any MT32 connection is made.

---

### Issue 5: Validate TDM timing and BCLK inversion implications

**Type:** Research  
**Priority:** High  
**Description:**  
Review the MT32 TDM output format (slot width, sample width, BCLK/LRCK polarity, data delay). Review the TLV320AIC3104 TDM input requirements. Determine whether the Teensy8x8AudioBoard BCLK inversion creates a mismatch when the MT32 drives BCLK. Document findings in `docs/tdm-format-open-questions.md`.

---

### Issue 6: Bring up Teensy8x8 board using original Teensy flow

**Type:** Hardware Task  
**Priority:** Medium  
**Description:**  
Following Stage 1 of the bring-up plan (`docs/bringup-plan.md`), bring up the Teensy8x8AudioBoard with a Teensy 4.x MCU using the original firmware libraries. Verify codec I2C communication, basic audio loopback, and TDM audio at 48 kHz. Document results.

---

### Issue 7: Create MT32 interposer schematic

**Type:** Hardware Task  
**Priority:** Medium  
**Description:**  
Using the verified pin map from issues #1–#3, create the first draft KiCad schematic for the MT32-FFC to Teensy8x8AudioBoard interposer. Place schematic in `hardware/interposer/kicad/`. Flag any pins that still need verification.

---

### Issue 8: Create input pad test board

**Type:** Hardware Task  
**Priority:** Low  
**Description:**  
Design and build a prototype balanced H-pad for the input pad targets in `docs/input-pad-design.md`. Test with a known +4 dBu and +18 dBu source. Measure insertion loss, CMRR, and frequency response. Document results.

---

### Issue 9: Define TRS/XLR wing population variants

**Type:** Hardware Task  
**Priority:** Medium  
**Description:**  
For each wing board variant (TRS input, TRS output, XLR input, XLR output, Combo input), confirm the exact BOM. Verify capacitor values, bleed resistor placement, and 47 Ω output protection resistor placement against the wing board schematics. Update `docs/wing-board-population.md`.

---

### Issue 10: Measure ADC clipping level and output maximum level

**Type:** Measurement Task  
**Priority:** Medium  
**Description:**  
Following Stage 4/5 of the bring-up plan, measure the ADC full-scale input level (dBu at clipping) and the DAC full-scale output level (dBu at 0 dBFS). Record results in `docs/test-equipment.md` and `docs/line-level-and-headroom.md`. Determine whether input pads are needed for target signal levels.
