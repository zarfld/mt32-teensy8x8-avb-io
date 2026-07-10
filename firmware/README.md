# Firmware Notes

This directory will contain codec initialisation notes, MT32 configuration snippets, and any future firmware for this project.

## Contents

- [`codec-init-notes.md`](codec-init-notes.md) — TLV320AIC3104 and PCA9546 initialisation notes.

## Important Note

The primary firmware for the Teensy8x8AudioBoard (for Stage 1 bring-up) lives in the upstream repositories:

- `external/control_TLV320AIC3104` — codec driver library
- `external/Teensy8x8AudioBoard` — board support and audio examples
- `external/TeensyAudio` — Teensy Audio library

Do not duplicate upstream firmware here. Only project-specific notes, patches, or MT32-specific initialisation sequences belong in this directory.
