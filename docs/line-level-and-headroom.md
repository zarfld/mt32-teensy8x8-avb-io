# Line Level and Headroom

## Units Policy

When discussing audio voltage levels, this project uses the following conventions:

- **dBu** — voltage level referenced to 0.7746 Vrms (the voltage that would deliver 1 mW into 600 Ω). Used for **unloaded audio voltage levels**. This is the standard unit for professional audio.
  - +4 dBu nominal = 1.228 Vrms
  - +24 dBu = 12.28 Vrms
- **dBV** — voltage level referenced to 1 Vrms. Used **only** when a level is explicitly referenced to 1 Vrms.
  - −10 dBV nominal (consumer) = 0.316 Vrms
- **dBm** — power level referenced to 1 mW into a **specified load impedance**. Use dBm **only when a load impedance is explicitly stated** (commonly 600 Ω). Do not use dBm to describe a voltage level without stating the load.
- **Do not mix dBu and dBm for the same line-level statement** without explicitly stating the load.

Where this document previously said "+10 dBu / +10 dBm class", read as:

> approximately +10 dBu-class voltage level — depending on loading and actual measurement; "+10 dBm" only applies if explicitly specified into a defined load (typically 600 Ω).

---

## Overview

This document summarises the working assumptions about analog signal levels and headroom in the current prototype. The Teensy8x8AudioBoard is a useful development and bring-up platform; it is not intended to be a final pro-line interface.

---

## Working Assumptions

### 1. The Teensy8x8AudioBoard Is a Bring-Up Platform, Not a +24 dBu Interface

The Teensy8x8AudioBoard and its TLV320AIC3104 codecs are designed for consumer-to-prosumer signal levels. The board does not provide the +24 dBu (or +18 dBu) headroom expected of pro-line studio or broadcast audio interfaces.

This is acceptable for prototype bring-up and functional testing. It is **not** acceptable for direct connection to professional +4 dBu nominal balanced line equipment that may have peaks at +18 or +24 dBu.

### 2. Base Documented Levels

Based on the TLV320AIC3104 datasheet and the Teensy8x8AudioBoard design:

- Output maximum level is roughly in the **approximately +10 dBu-class voltage level** (differential output), depending on load impedance and specific board configuration. ("+10 dBm" would only apply if explicitly specified into a defined 600 Ω load.)
- These figures are working estimates. Actual measurements must be taken from the board under test. See `docs/test-equipment.md` and `docs/bringup-plan.md`, Stage 5.

### 3. Codec Input Range Is Limited

The TLV320AIC3104 ADC has a fixed input full-scale voltage. Hot pro-line signals at +18 or +24 dBu will clip the ADC before any useful dynamic range is used.

For signals that routinely sit at **+4 dBu nominal balanced line** the codec may be usable with careful gain staging. For inputs that can reach +18 or +24 dBu peaks, an input attenuation pad is required before the ADC.

### 4. +4 dBu Nominal Balanced Line Is Likely Achievable

With appropriate gain staging and the differential inputs of the TLV320AIC3104, **+4 dBu nominal balanced line input** is a realistic target for this prototype. Peaks at +10–+14 dBu are likely within reach. Peaks at +18 or +24 dBu are not without an input pad.

### 5. Input Pads Are Required for Full Pro-Line Compliance

For hot pro-line sources (+18/+24 dBu peaks):

- A passive balanced H-pad or symmetric attenuator is the simplest solution.
- Target switch positions: 0 dB (bypass), approximately −12 dB, approximately −20 dB.
- See [`docs/input-pad-design.md`](input-pad-design.md) for the preliminary pad design notes.

### 6. Transformers Are Not the Default Level-Conversion Approach

Input or output transformers are **not** the default level-conversion solution in this design. Transformers are mainly considered for:

- Galvanic isolation (useful when ground loops are a concern).
- Mic/line interface cases where transformer character is desired.
- Phantom power distribution on XLR inputs.

Transformer-based designs are more expensive, heavier, and more complex than passive pad approaches for simple level shifting. They remain an option for a future pro-line version.

---

## Summary Table

| Scenario | Current prototype | Notes |
|---|---|---|
| +4 dBu nominal balanced output | Likely achievable | Verify by measurement |
| +4 dBu nominal balanced input | Likely achievable | Verify gain staging |
| +18 dBu peak input | Not achievable without pad | Requires input attenuation |
| +24 dBu peak input | Not achievable without pad | Requires input attenuation |
| +48 V phantom power | Not supported | Requires transformer or active front-end |
| Galvanic isolation | Not implemented | Transformer required if needed |

---

## Related Documents

- [`docs/input-pad-design.md`](input-pad-design.md)
- [`docs/output-stage-options.md`](output-stage-options.md)
- [`docs/wing-board-population.md`](wing-board-population.md)
- [`docs/test-equipment.md`](test-equipment.md)
