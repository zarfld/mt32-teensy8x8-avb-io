# Submodules

This document records the exact `git submodule add` commands needed to initialise the third-party submodules for this project.

Run these commands from the root of the repository:

```sh
git submodule add https://github.com/JOYNED-GmbH/joyned_mt32-evk external/joyned_mt32-evk
git submodule add https://github.com/palmerr23/Teensy8x8AudioBoard external/Teensy8x8AudioBoard
git submodule add https://github.com/palmerr23/control_TLV320AIC3104 external/control_TLV320AIC3104
git submodule add https://github.com/PaulStoffregen/Audio external/TeensyAudio
```

After running these commands, commit the `.gitmodules` file and the new `external/` directory entries:

```sh
git add .gitmodules external/
git commit -m "Add third-party submodules under external/"
```

---

## After Cloning This Repository

When cloning this repository for the first time, initialise and update all submodules:

```sh
git clone --recurse-submodules https://github.com/zarfld/mt32-teensy8x8-avb-io.git
```

Or, if already cloned:

```sh
git submodule update --init --recursive
```

---

## Submodule License Note

Each submodule retains its own license. See [`LICENSES.md`](../LICENSES.md) for details.

---

## Submodule Reference Table

| Path | Repository | Purpose |
|---|---|---|
| `external/joyned_mt32-evk` | <https://github.com/JOYNED-GmbH/joyned_mt32-evk> | MT32 EVK reference schematics and documentation |
| `external/Teensy8x8AudioBoard` | <https://github.com/palmerr23/Teensy8x8AudioBoard> | Teensy8x8AudioBoard hardware and firmware |
| `external/control_TLV320AIC3104` | <https://github.com/palmerr23/control_TLV320AIC3104> | TLV320AIC3104 codec Arduino library |
| `external/TeensyAudio` | <https://github.com/PaulStoffregen/Audio> | Teensy Audio library |
