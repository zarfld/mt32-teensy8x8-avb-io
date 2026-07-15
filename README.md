# mt32-teensy8x8-avb-io

MT32 Milan/AVB to TDM 8x8 audio I/O integration prototype

> ⚠️ This is not a production-ready hardware design. All schematics, pin mappings, and measurements are working assumptions under active verification.

---

## What Is This Project?

`mt32-teensy8x8-avb-io` is a documentation and prototype repository for building a Milan/AVB audio I/O system using:

- The **[JOYNED MT32](https://github.com/JOYNED-GmbH/joyned_mt32-evk)** Milan Endpoint module as the network and TDM/I2S master clock source.
- The **[palmerr23 Teensy8x8AudioBoard](https://github.com/palmerr23/Teensy8x8AudioBoard)** as the first 8-channel codec and connector platform, populated with TRS, XLR, or Combo wings.
- A custom **MT32 FFC-to-Teensy8x8AudioBoard interposer** that connects the MT32's 30-pin FFC system connector directly to the audio board's TDM/clock/I2C/reset pins, bypassing the Teensy microcontroller in the long run.

The Teensy microcontroller is used in the **initial bring-up stage** with the original Teensy8x8AudioBoard firmware ecosystem. It is **not** the intended long-term host once the MT32 can drive the codec bank directly.

---

## Architecture Summary

```text
Milan/AVB network
  └─> JOYNED MT32
        └─> MT32 30-pin FFC system connector
              └─> custom interposer (this project)
                    └─> Teensy8x8AudioBoard TDM/audio/control pins
                          └─> TLV320AIC3104 codec bank (×4, via PCA9546 I2C mux)
                                └─> IDC wing connectors
                                      └─> TRS / XLR / Combo wing boards
```

For full architecture detail see [`docs/architecture.md`](docs/architecture.md).

---

## First Target: 8×8 Bring-Up

The **first milestone** is a working 8-input / 8-output audio path between the MT32 and one Teensy8x8AudioBoard. A 16-channel (16×16) or higher-channel-count configuration is future work.

The Teensy8x8AudioBoard is an acceptable bring-up platform even though it does not provide +24 dBu pro-line headroom. Future work may include:

- Input attenuation pads for hot pro-line (+18/+24 dBu) sources.
- Active balanced line receivers.
- Higher-headroom output stages.
- A custom MT32 daughterboard replacing the interposer.
- A 1U Milan/AVB patchbay or rack I/O unit.

---

## Third-Party Projects (Submodules)

Third-party repositories are referenced as Git submodules under `external/`. They keep their own licenses; this repository does not relicense upstream material. See [`LICENSES.md`](LICENSES.md) and [`docs/submodules.md`](docs/submodules.md).

| Submodule path | Repository |
|---|---|
| `external/joyned_mt32-evk` | <https://github.com/JOYNED-GmbH/joyned_mt32-evk> |
| `external/Teensy8x8AudioBoard` | <https://github.com/palmerr23/Teensy8x8AudioBoard> |
| `external/control_TLV320AIC3104` | <https://github.com/palmerr23/control_TLV320AIC3104> |
| `external/TeensyAudio` | <https://github.com/PaulStoffregen/Audio> |

---

## Repository Layout

```text
.
├── README.md
├── LICENSES.md
├── docs/                  Project documentation and working notes
├── hardware/              Hardware design files (interposer, pads, stages, wings)
├── firmware/              Codec init notes and future firmware
├── scripts/               Helper scripts
└── .github/               Issue templates and CI workflows
```

---

## Long-Term Vision

The long-term goal of this project is to produce a compact 1U (or similar form-factor) Milan/AVB patchbay or I/O unit with professional-grade analog levels, built around the JOYNED MT32 as the network endpoint. The Teensy8x8AudioBoard ecosystem is the first stepping stone toward that goal.

---

## Status

🚧 **Pre-alpha — structure and documentation scaffold only.** No verified schematics, no measured values, no PCB layouts yet.
