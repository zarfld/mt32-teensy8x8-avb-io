# XLR Output Wing Notes

## Purpose

The XLR Output wing provides 8 balanced XLR line-level outputs connected from the Teensy8x8AudioBoard IDC connector.

## Population

See [`docs/wing-board-population.md`](../../docs/wing-board-population.md), section **XLR Output Wing**, for the full component population table.

## Key Points

- 8× XLR-M (male) panel connectors (pin 2 = hot, pin 3 = cold, pin 1 = GND).
- 10 µF AC coupling capacitors (≥ 50 V) on each differential output leg.
- 47 Ω output protection resistors in series on each output leg (do not omit).
- No bleed resistors (output path only).

## Notes

- Same component strategy as TRS Output Wing; only the connectors differ.
- Verify XLR-M connector footprint matches PCB pattern.
