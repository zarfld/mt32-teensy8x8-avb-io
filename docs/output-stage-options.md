# Output Stage Options

## Status

🚧 **Placeholder — not yet designed. This document will record options and constraints for future output stage upgrades.**

---

## Current State

The Teensy8x8AudioBoard uses the TLV320AIC3104 codec DAC outputs directly, driving the wing board connector through the onboard output path. This provides a consumer-to-prosumer output level (approximately +10 dBu class differential, load dependent — see [`docs/line-level-and-headroom.md`](line-level-and-headroom.md)).

---

## Potential Upgrade Paths

### Option A: Onboard Gain Stage

- Add an op-amp-based balanced output driver between the codec DAC output and the output connector.
- Could provide +6 to +12 dB of additional gain to push output closer to +18/+24 dBu.
- Requires op-amp selection for low noise, low distortion, and ±15 V rail capability (which the current board may not supply).

### Option B: Active Balanced Line Driver Wing

- Add a dedicated output wing board with a balanced line driver IC (e.g., THAT1646, DRV134, or similar).
- Higher headroom, better drive capability, easier to optimise independently.
- More complex wing board; may need additional power supply rails.

### Option C: Output Transformer

- Add a small output transformer per channel for galvanic isolation or level shifting.
- Heavier, more expensive, and frequency-response compromises.
- May be appropriate for specific use cases (e.g., broadcast patch bay, cable-distance isolation).

---

## Future Decisions Needed

- Confirm target output level (e.g., +18 dBu or +24 dBu).
- Confirm power supply availability on output stage board.
- Select op-amp or line driver IC.
- Decide whether output stage is part of a modified wing board or a separate auxiliary board.

---

## Related Documents

- [`docs/line-level-and-headroom.md`](line-level-and-headroom.md)
- [`docs/input-pad-design.md`](input-pad-design.md)
- [`docs/wing-board-population.md`](wing-board-population.md)
- [`hardware/output-stage/README.md`](../hardware/output-stage/README.md)
