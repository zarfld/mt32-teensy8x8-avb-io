# XLR Input Wing Notes

## Purpose

The XLR Input wing provides 8 balanced XLR line-level inputs connected to the Teensy8x8AudioBoard IDC connector.

## Population

See [`docs/wing-board-population.md`](../../docs/wing-board-population.md), section **XLR Input Wing**, for the full component population table.

## Key Points

- 8× XLR-F (female) panel connectors (pin 2 = hot, pin 3 = cold, pin 1 = GND).
- 1 µF non-polarised AC coupling capacitors (≥ 50 V) on each differential input leg.
- 470 kΩ bleed resistors to bias floating inputs.
- No phantom power support on this variant.
- No 47 Ω output protection resistors (input path only).

## Open Questions

- Verify XLR-F connector footprint matches PCB pattern.
- Confirm no phantom power bleeds from other sources onto this wing.

## Future Variant

For microphone inputs requiring +48 V phantom power, see the Phantom XLR Input Wing (future, not yet documented as hardware).
