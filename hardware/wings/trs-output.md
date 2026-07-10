# TRS Output Wing Notes

## Purpose

The TRS Output wing provides 8 balanced TRS line-level outputs connected from the Teensy8x8AudioBoard IDC connector.

## Population

See [`docs/wing-board-population.md`](../../docs/wing-board-population.md), section **TRS Output Wing**, for the full component population table.

## Key Points

- 8× balanced TRS female jacks (stereo, tip = hot, ring = cold, sleeve = GND).
- 100 µF AC coupling capacitors (≥ 10 V) preferred on each differential output leg for better low-frequency response into lower impedance loads.
- 47 Ω output protection resistors in series on each output leg (do not omit).
- Bleed resistor placement and populate/omit status must be checked against schematic/silkscreen — do not assume.

## Notes

- 100 µF (preferred) vs 10 µF: larger value improves bass extension into lower-impedance loads; ≥ 10 V is electrically sufficient unless another design constraint requires more.
- 47 Ω output protection limits short-circuit current from the TLV320AIC3104 DAC output.
