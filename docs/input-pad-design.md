# Input Pad Design Notes

## Status

🚧 **Placeholder — not yet designed. This document records the design constraints and intent for future balanced input pads.**

---

## Purpose

To allow the Teensy8x8AudioBoard / TLV320AIC3104 codec bank to accept hot pro-line signals (+18 to +24 dBu) without ADC clipping, a passive balanced input attenuation pad is required in the signal path ahead of the codec differential input.

---

## Design Constraints

### Pad Topology

- **Prefer a balanced H-pad** (symmetric resistor network with equal series elements on both legs and a shunt element between them) to maintain common-mode rejection and preserve balanced signal integrity.
- A symmetric high-impedance balanced pad (minimising loading of the source) is preferred over an asymmetric or unbalanced attenuator.
- Do not use simple voltage dividers that break symmetry — these degrade CMRR.

### Target Attenuation Switch Positions

| Position | Nominal Attenuation | Intended Use |
|---|---|---|
| 0 | 0 dB (bypass) | +4 dBu nominal consumer / prosumer line |
| 1 | ≈ −12 dB | Semi-pro / broadcast line levels |
| 2 | ≈ −20 dB | Full pro-line +24 dBu peaks |

Exact resistor values should be calculated once the codec input impedance, source impedance, and desired insertion loss are confirmed.

---

## Important: Existing Board Resistors Are Not the Pad

> ⚠️ Do not confuse existing resistors on the Teensy8x8AudioBoard and wing boards with the input pad.

- The **470 kΩ bleed resistors** on the input wing boards serve to discharge DC bias and bias the differential input to mid-rail. They are **not** part of any attenuation pad.
- The **47 Ω output protection resistors** on output paths limit short-circuit current from the codec output stage. They are **not** part of any input pad and are irrelevant to input attenuation.

---

## Wing Board Series Insertion Points

The TRS Wing boards have series component footprints (R9–R16) that may be usable as series insertion points for a balanced attenuator. However:

- A complete H-pad also requires **shunt elements** between the two balanced legs.
- The wing board layout may not have footprints for the shunt elements.
- A complete balanced input pad likely requires a small auxiliary PCB or modified wing board.

This needs to be verified against the wing board schematic and layout.

---

## Next Steps

1. Obtain Teensy8x8AudioBoard TRS input wing schematic.
2. Confirm R9–R16 footprint locations and series insertion compatibility.
3. Design H-pad for −12 dB and −20 dB targets.
4. Select appropriate resistor values for 10 kΩ–20 kΩ source impedance / 10 kΩ load (codec input).
5. Build and measure prototype pad on a small perf board or auxiliary PCB.
6. Verify CMRR before and after pad insertion.

---

## Related Documents

- [`docs/line-level-and-headroom.md`](line-level-and-headroom.md)
- [`docs/wing-board-population.md`](wing-board-population.md)
- [`docs/output-stage-options.md`](output-stage-options.md)
- [`hardware/input-pad/README.md`](../hardware/input-pad/README.md)
