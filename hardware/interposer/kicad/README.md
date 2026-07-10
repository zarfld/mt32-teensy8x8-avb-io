# KiCad Interposer Project

🚧 **Placeholder — KiCad project not yet created.**

## Purpose

This directory will contain the KiCad schematic and PCB layout for the MT32 FFC to Teensy8x8AudioBoard interposer.

## Prerequisites Before Starting

1. Verify pin map (`docs/interposer-pinmap.md`) against physical boards.
2. Resolve BCLK inversion question (`docs/tdm-format-open-questions.md`).
3. Confirm power strategy (`docs/bringup-plan.md`, Stage 0).

## Planned Files

```text
hardware/interposer/kicad/
├── mt32-teensy8x8-interposer.kicad_pro
├── mt32-teensy8x8-interposer.kicad_sch
├── mt32-teensy8x8-interposer.kicad_pcb
└── README.md
```

## Notes

- Use KiCad 7 or later.
- The interposer is passive (or near-passive): primarily wire routing, possibly a BCLK inversion gate if needed.
- FFC connector footprint for the MT32 30-pin FFC must be verified against the actual connector pitch and lock type.
- Board thickness and FFC connector mounting should be compatible with the MT32 EVK mechanical envelope.
