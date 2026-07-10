# System Architecture

## High-Level Block Diagram

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

---

## Signal Flow

### Audio (downstream / playback)

```text
AVB network frame
  -> MT32 internal DSP / router
  -> TDM/I2S frame on FFC SDIOB_OUT (playback data to codec DAC)
  -> interposer
  -> Teensy8x8AudioBoard DI pin
  -> TLV320AIC3104 codec DAC
  -> wing board output jacks (TRS balanced / XLR / Combo)
```

### Audio (upstream / capture)

```text
Wing board input jacks (TRS balanced / XLR / Combo)
  -> TLV320AIC3104 codec ADC
  -> Teensy8x8AudioBoard DO pin
  -> interposer
  -> MT32 FFC SDIOD_IN
  -> MT32 internal DSP / router
  -> AVB network frame
```

### Clock

```text
MT32 MCLK_OUT (FFC pin 21)
  -> interposer
  -> Teensy8x8AudioBoard MCLK pin

MT32 SCLK_OUT / BCLK (FFC pin 17)
  -> interposer
  -> Teensy8x8AudioBoard BCLK pin
  (note: board may invert — see docs/tdm-format-open-questions.md)

MT32 LRCK_OUT / SYNC (FFC pin 19)
  -> interposer
  -> Teensy8x8AudioBoard LRCK/SYNC pin
```

### Control (I2C)

```text
MT32 SDA (FFC pin 23) / SCL (FFC pin 24)
  -> interposer
  -> Teensy8x8AudioBoard I2C header
  -> PCA9546A I2C mux
  -> TLV320AIC3104 codec #0 (channel 0)
  -> TLV320AIC3104 codec #1 (channel 1)
  -> TLV320AIC3104 codec #2 (channel 2)
  -> TLV320AIC3104 codec #3 (channel 3)
```

### Reset / GPIO

```text
MT32 ADC_RST_N_OUT (FFC pin 6)
  -> interposer
  -> Teensy8x8AudioBoard reset / GPIO22 equivalent
  -> codec reset (active-low)
```

---

## Power

The Teensy8x8AudioBoard requires 5 V (from Teensy USB or external) and provides 3.3 V via onboard regulator. The MT32 FFC also exposes 5 V and 3.3 V rails.

> ⚠️ **Power strategy requires verification before any connection is made.** Do not assume the MT32 can source sufficient current for the codec board. See `docs/bringup-plan.md`, Stage 0.

---

## Future Architecture (Long-Term)

A custom MT32 daughterboard may eventually replace the interposer and the Teensy PCB, connecting the MT32 FFC directly to a purpose-built codec board with:

- Pro-line input attenuation pads (see `docs/input-pad-design.md`).
- Higher-headroom output stages (see `docs/output-stage-options.md`).
- Balanced XLR I/O with optional phantom power.
- A 1U or 2U rack form factor.
