# TRS Input Wing Notes

## Purpose

The TRS Input wing provides 8 balanced TRS line-level inputs connected to the Teensy8x8AudioBoard IDC connector.

## Population

See [`docs/wing-board-population.md`](../../docs/wing-board-population.md), section **TRS Input Wing**, for the full component population table.

## Key Points

- 8× balanced TRS female jacks (stereo, tip = hot, ring = cold, sleeve = GND).
- 1 µF non-polarised AC coupling capacitors (≥ 50 V) on each differential input leg.
- 470 kΩ bleed resistors to bias floating inputs.
- Series footprints (R9–R16 or equivalent) reserved for future input pad insertion.
- No 47 Ω output protection resistors (input path only).

## Open Questions

- Exact R9–R16 position and layout on the physical PCB.
- Whether the series footprints can accommodate a complete balanced H-pad or only the series elements.
