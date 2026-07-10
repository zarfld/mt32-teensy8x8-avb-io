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
- A symmetric high-impedance balanced attenuator is preferred over an asymmetric or unbalanced attenuator.
- Do not use simple voltage dividers that break symmetry — these degrade CMRR.

### Electrical Assumptions Required Before Designing

> ⚠️ **Do not calculate final resistor values until the following are confirmed and explicitly documented as assumptions:**
>
> - Effective codec input impedance (depends on selected TLV320AIC3104 input mode and PGA setting — verify from datasheet for chosen configuration).
> - Selected codec input mode (differential, single-ended, etc.) and its effect on input impedance.
> - Selected PGA setting and its effect on input impedance.
> - Coupling capacitor value (affects low-frequency response, especially for shunt elements in H-pad).
> - Desired input impedance of the complete pad.
> - Source impedance of the intended signal source.

### Source Impedance Model

- Typical modern professional line outputs are **low source impedance voltage sources** (a few tens of ohms to a few hundred ohms typically), not 600 Ω matched sources.
- Do not assume 600 Ω source impedance or design for 600 Ω termination unless a 600 Ω matched system is explicitly required and documented.
- Do not assume a fixed 10 kΩ–20 kΩ source impedance or 10 kΩ codec load — these must be derived from the actual codec datasheet and operating mode.
- Initial design target: treat the source as a **low-impedance balanced voltage source**; design the pad for high input impedance to minimise loading.

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
3. Confirm shunt element footprint availability (a complete H-pad requires shunt elements in addition to the series elements at R9–R16).
4. Confirm effective codec input impedance, selected input mode, and PGA setting from the TLV320AIC3104 datasheet for the intended operating configuration.
5. Confirm source impedance of the intended signal source.
6. Design H-pad for −12 dB and −20 dB targets once all electrical assumptions are documented.
7. Build and measure prototype pad on a small perf board or auxiliary PCB.
8. Verify CMRR before and after pad insertion.

---

## Related Documents

- [`docs/line-level-and-headroom.md`](line-level-and-headroom.md) — includes units policy for dBu, dBV, and dBm
- [`docs/wing-board-population.md`](wing-board-population.md)
- [`docs/output-stage-options.md`](output-stage-options.md)
- [`hardware/input-pad/README.md`](../hardware/input-pad/README.md)
