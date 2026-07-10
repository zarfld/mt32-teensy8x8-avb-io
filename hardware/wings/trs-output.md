# TRS Output Wing Notes

## Purpose

The TRS Output wing provides 8 balanced TRS line-level outputs connected from the Teensy8x8AudioBoard IDC connector.

## Population

See [`docs/wing-board-population.md`](../../docs/wing-board-population.md), section **TRS Output Wing**, for the full component population table.

## Key Points

- 8× balanced TRS female jacks (stereo, tip = hot, ring = cold, sleeve = GND).
- 10 µF AC coupling capacitors (≥ 50 V) on each differential output leg.
- 47 Ω output protection resistors in series on each output leg (do not omit).
- No bleed resistors (output path only).

## Notes

- Higher capacitor value (10 µF vs 1 µF on input) helps bass extension into low-impedance loads.
- 47 Ω output protection limits short-circuit current from the TLV320AIC3104 DAC output.
