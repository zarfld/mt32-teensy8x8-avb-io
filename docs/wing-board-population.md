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
| Coupling capacitors | 1 µF or 10 µF, ≥ 10 V (film or X7R ceramic; 50 V acceptable if mechanically fitting) | C1–C8 (each channel hot+cold) | AC coupling, DC blocking |
| Bleed resistors | 470 kΩ | R1–R8 or equivalent | Discharge DC bias on floating inputs |
| Output protection resistors | **Omitted / linked (0 Ω)** | Series position on output lines | Not needed on input wing |
| Series insertion footprints | R9–R16 (0 Ω / DNP) | Signal path series position | Reserve for future input pad series element — see `docs/input-pad-design.md`; these are series insertion points only, not a complete pad |

### Notes

- 470 kΩ bleed resistors bias the differential input to mid-rail when no source is connected.
- Coupling capacitors: 1 µF is sufficient for line-level inputs; 10 µF acceptable for conservative low-frequency margin.
- 50 V rating is acceptable if mechanically fitting, but is not required for normal TRS line-level input; ≥ 10 V is the minimum electrical requirement.
- Normal non-phantom TRS line inputs do not require non-polarised 50 V capacitors unless specifically called out in the source files.
- The series footprints (R9–R16) allow future insertion of a series element for a balanced attenuator. A complete H-pad also requires shunt element(s) — verify against wing board layout.

---

## TRS Output Wing

| Component | Value / Part | Location | Purpose |
|---|---|---|---|
| Coupling capacitors | 100 µF preferred (electrolytic, ≥ 10 V) | C1–C8 (each channel hot+cold) | AC coupling, DC blocking on output; larger value for better LF response |
| Bleed resistors | Check schematic/silkscreen before populating or omitting | — | Behaviour depends on actual board design — verify before deciding |
| Output protection resistors | 47 Ω | R1–R8 (series on hot+cold) | Limit short-circuit current from codec output |

### Notes

- 47 Ω output protection resistors are standard on the codec DAC output path. Do not omit.
- 100 µF coupling capacitors preferred for output: larger value reduces bass roll-off into low-impedance loads; 10 V or greater is electrically sufficient unless another design constraint requires more.
- Bleed resistor placement and populate/omit status must be checked against schematic/silkscreen — do not state definitively without that verification.

---

## XLR Input Wing

| Component | Value / Part | Location | Purpose |
|---|---|---|---|
| Coupling capacitors | 1 µF or 10 µF, ≥ 10 V (film or X7R ceramic; 50 V only required where phantom power may appear) | C1–C8 per channel | AC coupling |
| Bleed resistors | 470 kΩ | R1–R8 per channel | Bias floating inputs |
| Output protection resistors | **Omitted / linked** | — | Not needed on input wing |
| Phantom blocking capacitors | **Not populated** | — | No phantom power support on this variant |

### Notes

- No phantom power on this variant. Use Phantom XLR Input Wing for microphone inputs.
- Normal non-phantom XLR line inputs do not require non-polarised 50 V capacitors; ≥ 10 V is the minimum electrical requirement.
- 50 V capacitors are only required where phantom power (+48 V) may appear on the line.
- XLR-F connectors: verify panel punch dimensions and IDC/PCB footprint.

---

## XLR Output Wing

| Component | Value / Part | Location | Purpose |
|---|---|---|---|
| Coupling capacitors | 100 µF preferred (electrolytic, ≥ 10 V) | C1–C8 per channel | AC coupling on output |
| Bleed resistors | Check schematic/silkscreen before populating or omitting | — | Behaviour depends on actual board design — verify before deciding |
| Output protection resistors | 47 Ω placement must be verified against schematic/silkscreen | R1–R8 per channel | Short-circuit protection |

### Notes

- XLR-M connectors: verify footprint.
- 47 Ω output protection resistor placement must be verified against schematic/silkscreen before asserting populate/omit.
- Same general component strategy as TRS Output Wing; connectors differ.

---

## Combo Input Wing

| Component | Value / Part | Location | Purpose |
|---|---|---|---|
| Coupling capacitors | 1 µF or 10 µF, ≥ 10 V (film or X7R ceramic) | C1–C8 per channel | AC coupling |
| Bleed resistors | 470 kΩ | R1–R8 per channel | Bias floating inputs |
| Output protection resistors | **Omitted / linked** | — | Not needed on input wing |

### Notes

- Input-only wing.
- Combo XLR/TRS jacks: verify panel footprint and switching contacts.
- Switching contacts on combo jacks may route TRS or XLR signal — verify which is normalised.
- Normal non-phantom combo inputs do not require 50 V capacitors; ≥ 10 V is the minimum electrical requirement.

---

## Phantom XLR Input Wing (Future)

| Component | Value / Part | Location | Purpose |
|---|---|---|---|
| Phantom feed resistors | ≈ 6.8 kΩ (verify against schematic for exact value) | R_phantom per channel | +48 V phantom current limit for 48 V operation |
| Phantom blocking capacitors | ≥ 50 V (electrolytic or film) | C_phantom per channel | Block +48 V from codec input — 50 V or higher required |
| Bleed resistors | 470 kΩ | R_bleed per channel | Bias floating inputs |
| Output protection resistors | **Omitted / linked** | — | Not needed on input |

### Notes

- Phantom power (+48 V) requires a dedicated supply rail not present on the current Teensy8x8AudioBoard.
- ≥ 50 V capacitor rating is required for phantom blocking (50 V or higher).
- Phantom feed resistor value approximately 6.8 kΩ for 48 V operation — verify against schematic before ordering.
- This wing variant is **not** part of the first milestone. It is listed here for future reference only.

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
