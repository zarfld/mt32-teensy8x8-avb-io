# Interposer

This directory contains design files for the custom MT32 FFC to Teensy8x8AudioBoard interposer.

## Purpose

The interposer is a passive (or minimally active) adapter PCB that:

- Connects the JOYNED MT32 30-pin FFC system connector to the Teensy8x8AudioBoard expansion header.
- Routes TDM/I2S audio clocks and data (MCLK, BCLK, LRCK, DI, DO).
- Routes I2C control bus (SDA, SCL) for codec initialisation.
- Routes codec reset / GPIO signal.
- Routes power (5 V, 3.3 V, GND) — subject to power strategy verification.

## Power Safety Rule

> ⚠️ **The initial interposer revision must not directly connect MT32 5 V or 3.3 V rails to the Teensy8x8AudioBoard by default.**
>
> Use jumpers, solder bridges, or DNP (Do Not Populate) links for optional power connection only — and only after explicit verification.
>
> Initial safe bring-up:
> - Teensy8x8AudioBoard powered from a separate current-limited bench 5 V supply.
> - MT32 and Teensy8x8 grounds connected only after checking for shorts and ground strategy.
> - MT32 5 V and 3.3 V left unconnected unless explicitly enabled by a verified jumper strategy.
> - No back-feeding of regulators.

## Status

🚧 **Not designed yet.** The pin mapping is in draft stage. See [`docs/interposer-pinmap.md`](../../docs/interposer-pinmap.md).

## Directory Layout

```text
hardware/interposer/
├── README.md          (this file)
├── pinmap/
│   └── mt32-to-teensy8x8-pinmap.csv   Machine-readable pin map
└── kicad/
    └── README.md      KiCad project placeholder
```

## Next Steps

1. Verify pin map (see GitHub issues #1, #2, #3).
2. Confirm power strategy (see GitHub issue #4).
3. Resolve BCLK inversion question (see GitHub issue #5).
4. Create KiCad schematic in `kicad/`.
