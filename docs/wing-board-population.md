# Wing Board Population Guide

This document provides a population reference for the various wing board variants used with the Teensy8x8AudioBoard.

> ⚠️ **This table is a working draft. All values must be verified against the wing board schematics and PCB silkscreen before assembly. Do not use this as a sole assembly reference.**

---

## Wing Board Summary

| Wing Board | Purpose | Connectors Needed | Notes |
|---|---|---|---|
| TRS Input | Balanced line-level input (TRS) | 8× TRS female jacks (stereo/balanced) | Input only |
| TRS Output | Balanced line-level output (TRS) | 8× TRS female jacks (stereo/balanced) | Output only |
| XLR Input | Balanced line-level input (XLR) | 8× XLR-F panel connectors | Input only; no phantom power |
| XLR Output | Balanced line-level output (XLR) | 8× XLR-M panel connectors | Output only |
| Combo Input | Combined XLR+TRS input (combo jacks) | 8× Combo XLR/TRS female jacks | Input only |
| Phantom XLR Input | Balanced mic input with +48 V phantom | 8× XLR-F panel connectors | Future; requires phantom supply |

---

## TRS Input Wing

| Component | Value / Part | Location | Purpose |
|---|---|---|---|
| Coupling capacitors | 1 µF (non-polarised, ≥ 50 V) | C1–C8 (each channel hot+cold) | AC coupling, DC blocking |
| Bleed resistors | 470 kΩ | R1–R8 or equivalent | Discharge DC bias on floating inputs |
| Output protection resistors | **Omitted / linked (0 Ω)** | Series position on output lines | Not needed on input wing |
| Series insertion footprints | R9–R16 (0 Ω / DNP) | Signal path series position | Reserve for future input pad — see `docs/input-pad-design.md` |

### Notes
- 470 kΩ bleed resistors bias the differential input to mid-rail when no source is connected.
- 1 µF coupling capacitors: use film or X7R ceramic rated ≥ 50 V for low distortion.
- The series footprints (R9–R16 or equivalent) may allow future insertion of a balanced H-pad without PCB modification.

---

## TRS Output Wing

| Component | Value / Part | Location | Purpose |
|---|---|---|---|
| Coupling capacitors | 10 µF (electrolytic or film, ≥ 50 V) | C1–C8 (each channel hot+cold) | AC coupling, DC blocking on output |
| Bleed resistors | **Omitted / DNP** | — | Not applicable to output path |
| Output protection resistors | 47 Ω | R1–R8 (series on hot+cold) | Limit short-circuit current from codec output |

### Notes
- 47 Ω output protection resistors are standard on the codec DAC output path. Do not omit.
- 10 µF coupling capacitors on output: higher value reduces bass roll-off into low-impedance loads. Verify low-frequency response with target load.

---

## XLR Input Wing

| Component | Value / Part | Location | Purpose |
|---|---|---|---|
| Coupling capacitors | 1 µF (non-polarised, ≥ 50 V) | C1–C8 per channel | AC coupling |
| Bleed resistors | 470 kΩ | R1–R8 per channel | Bias floating inputs |
| Output protection resistors | **Omitted / linked** | — | Not needed on input wing |
| Phantom blocking capacitors | **Not populated** | — | No phantom power support on this variant |

### Notes
- No phantom power on this variant. Use Phantom XLR Input Wing for microphone inputs.
- XLR-F connectors: verify panel punch dimensions and IDC/PCB footprint.

---

## XLR Output Wing

| Component | Value / Part | Location | Purpose |
|---|---|---|---|
| Coupling capacitors | 10 µF (electrolytic or film, ≥ 50 V) | C1–C8 per channel | AC coupling on output |
| Bleed resistors | **Omitted / DNP** | — | Not applicable to output path |
| Output protection resistors | 47 Ω | R1–R8 per channel | Short-circuit protection |

### Notes
- XLR-M connectors: verify footprint.
- Same component strategy as TRS Output Wing; connectors differ.

---

## Combo Input Wing

| Component | Value / Part | Location | Purpose |
|---|---|---|---|
| Coupling capacitors | 1 µF (non-polarised, ≥ 50 V) | C1–C8 per channel | AC coupling |
| Bleed resistors | 470 kΩ | R1–R8 per channel | Bias floating inputs |
| Output protection resistors | **Omitted / linked** | — | Not needed on input wing |

### Notes
- Combo XLR/TRS jacks: verify panel footprint and switching contacts.
- Switching contacts on combo jacks may route TRS or XLR signal — verify which is normalised.

---

## Phantom XLR Input Wing (Future)

| Component | Value / Part | Location | Purpose |
|---|---|---|---|
| Phantom feed resistors | 6.81 kΩ (typical) | R_phantom per channel | +48 V phantom current limit |
| Phantom blocking capacitors | 10 µF, 100 V (electrolytic or film) | C_phantom per channel | Block +48 V from codec input |
| Bleed resistors | 470 kΩ | R_bleed per channel | Bias floating inputs |
| Output protection resistors | **Omitted / linked** | — | Not needed on input |

### Notes
- Phantom power (+48 V) requires a dedicated ±48 V supply rail not present on the current Teensy8x8AudioBoard.
- 100 V rated capacitors required for phantom blocking.
- This wing variant is **not** part of the first milestone. It is listed here for future reference.

---

## General Notes

- "DNP" = Do Not Populate (component footprint present but component not installed).
- "Linked" = Replace component with 0 Ω link or solder bridge.
- All capacitor voltage ratings should have ≥ 2× safety margin above the maximum expected voltage.
- Verify all footprints against the actual wing board PCB before ordering parts.

---

## Related Documents

- [`docs/line-level-and-headroom.md`](line-level-and-headroom.md)
- [`docs/input-pad-design.md`](input-pad-design.md)
- [`docs/output-stage-options.md`](output-stage-options.md)
- [`hardware/wings/README.md`](../hardware/wings/README.md)
