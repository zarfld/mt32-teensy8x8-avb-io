# Bring-Up Plan

This document describes the staged bring-up plan for the MT32 / Teensy8x8AudioBoard integration prototype.

> All stages should be performed with appropriate bench-test precautions: current-limited power supplies, oscilloscope or multimeter monitoring, and no expensive equipment connected until prior stages are verified.

---

## Stage 0: Board Inspection and Power Verification

**Goal:** Confirm boards are correctly assembled and safe to power.

> ⚠️ **Power safety rule:** Do not connect MT32 5 V or 3.3 V rails to the Teensy8x8AudioBoard by default. The initial interposer must use jumpers, solder bridges, or DNP links for any power connection — enabled only after explicit verification. See `docs/interposer-pinmap.md`, Power Strategy Notes.

- [ ] Visually inspect Teensy8x8AudioBoard for solder bridges, missing components, or reversed SMD parts.
- [ ] Verify SMD component orientations (ICs, electrolytic capacitors, diodes).
- [ ] Verify jumper and solder bridge settings against the board documentation.
- [ ] Check for shorts between power rails (5 V / 3.3 V / GND) with a multimeter (power off).
- [ ] Apply 5 V to the board from a **current-limited bench power supply** (start with ≤ 200 mA limit).
- [ ] Verify 3.3 V rail comes up correctly.
- [ ] Verify no excessive current draw (idle current should be a few tens of mA without MCU or audio active).
- [ ] Connect MT32 and Teensy8x8 **grounds only after checking for shorts** and confirming the ground strategy.
- [ ] Leave MT32 5 V and 3.3 V **unconnected** until verified — no back-feeding of regulators.
- [ ] Do **not** connect any external audio equipment or the MT32 power rails at this stage.

---

## Stage 1: Teensy8x8AudioBoard with Teensy MCU (Original Flow)

**Goal:** Verify the Teensy8x8AudioBoard works correctly with its intended Teensy microcontroller before adding MT32 complexity.

- [ ] Insert Teensy 4.x into the Teensy socket.
- [ ] Install the Arduino IDE with Teensyduino and the required libraries (`external/control_TLV320AIC3104`, `external/Teensy8x8AudioBoard`, `external/TeensyAudio`).
- [ ] Flash a basic codec initialisation and audio example sketch.
- [ ] Verify codec responds to I2C commands (check for I2C ACK on codec addresses after mux select).
- [ ] Verify audio loopback or tone output with oscilloscope or audio analyser on output wing connectors.
- [ ] Verify basic TDM audio (8×8 I/O) at 48 kHz.
- [ ] Document any deviations from expected behaviour.

---

## Stage 2: Passive MT32 FFC to Teensy8x8AudioBoard Interposer Build

**Goal:** Build and safety-check the interposer before any live connections.

- [ ] Build the interposer according to the draft pin map in [`docs/interposer-pinmap.md`](interposer-pinmap.md).
- [ ] Verify all interposer connections with a continuity tester (power off, cables disconnected).
- [ ] Verify no shorts between signal pins and power/ground.
- [ ] Verify no shorts between MT32-side and Teensy8x8-side power rails.
- [ ] Remove the Teensy MCU from the Teensy8x8AudioBoard (the MT32 will drive the board directly in later stages).
- [ ] Connect the interposer to the Teensy8x8AudioBoard expansion header with **no MT32 connected** and re-check for shorts.
- [ ] With current-limited supply, power the Teensy8x8AudioBoard (no MT32, no audio).
- [ ] Verify no unexpected current draw or voltage on interposer signal lines.
- [ ] Connect the MT32 side of the interposer to the MT32 FFC connector with **MT32 powered off**.
- [ ] Final continuity and short check with full assembly before first power.

---

## Stage 3: Clock and Data Verification

**Goal:** Confirm MT32 clocks reach the codec bank and TDM data is correctly formatted.

- [ ] Power up MT32 and Teensy8x8AudioBoard via interposer.
- [ ] Monitor 5 V and 3.3 V rails under load; verify no voltage sag or overcurrent.
- [ ] Verify MCLK arrives at the Teensy8x8AudioBoard MCLK pin (oscilloscope).
- [ ] Verify BCLK arrives at the board BCLK pin with correct frequency and polarity.
- [ ] Verify LRCK/SYNC arrives at the board LRCK pin with correct framing.
- [ ] Determine whether BCLK inversion is required (see [`docs/tdm-format-open-questions.md`](tdm-format-open-questions.md)).
- [ ] Verify or implement MT32 codec initialisation via I2C (see open questions doc).
- [ ] Confirm codec is not in reset state; verify I2C communication to PCA9546 and codec ICs.
- [ ] Capture TDM data on logic analyser and verify slot/sample format.

---

## Stage 4: Audio Path Verification

**Goal:** Confirm end-to-end audio I/O.

- [ ] Verify one DAC output channel with a sine wave tone from the AVB network (oscilloscope or audio analyser on output TRS/XLR).
- [ ] Extend to all 8 DAC output channels.
- [ ] Verify one ADC input channel (inject a known tone on an input connector; verify it appears in the AVB stream).
- [ ] Extend to all 8 ADC input channels.
- [ ] Verify full-duplex operation (simultaneous 8×8 I/O).
- [ ] Measure output level and THD+N at a representative level.
- [ ] Measure ADC clipping level.
- [ ] Document results in [`docs/test-equipment.md`](test-equipment.md).

---

## Stage 5: Level and Headroom Evaluation

**Goal:** Characterise analog levels and determine whether input pads and/or output stage upgrades are needed.

- [ ] Measure maximum output level (0 dBFS full scale) in dBu.
- [ ] Measure ADC full-scale input level in dBu.
- [ ] Assess whether +4 dBu nominal balanced line is achievable with current gain structure.
- [ ] Evaluate need for input attenuation pads for pro-line (+18/+24 dBu) sources.
- [ ] Evaluate need for output stage upgrade for pro-line output levels.
- [ ] Document findings in [`docs/line-level-and-headroom.md`](line-level-and-headroom.md).
- [ ] Create input pad prototype if needed (see [`docs/input-pad-design.md`](input-pad-design.md)).

---

## Related Documents

- [`docs/project-overview.md`](project-overview.md)
- [`docs/architecture.md`](architecture.md)
- [`docs/interposer-pinmap.md`](interposer-pinmap.md)
- [`docs/tdm-format-open-questions.md`](tdm-format-open-questions.md)
- [`docs/test-equipment.md`](test-equipment.md)
