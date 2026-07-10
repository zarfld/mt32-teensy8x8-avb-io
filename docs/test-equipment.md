# Test Equipment

## Purpose

This document records the test equipment available for the prototype bring-up and the measurements required.

---

## Required Equipment

| Equipment | Purpose | Required From Stage |
|---|---|---|
| Digital multimeter | Continuity, voltage, and short checks | Stage 0 |
| Bench power supply (current-limited) | Safe first power-up | Stage 0 |
| Oscilloscope (≥ 100 MHz) | Clock, data, and analog signal verification | Stage 2 |
| Logic analyser (SPI/I2C decode capable) | TDM frame capture and I2C decode | Stage 3 |
| Audio signal generator | Known-level sine wave for ADC test | Stage 4 |
| Audio analyser or DAW with metering | Level, THD+N measurement | Stage 4/5 |

---

## Measurements to Record

| Measurement | Target / Expected | Actual (fill in after test) | Notes |
|---|---|---|---|
| 5 V rail under load | 5.0 V ± 5% | — | Stage 0/2 |
| 3.3 V rail under load | 3.3 V ± 5% | — | Stage 0/2 |
| MCLK frequency | 12.288 or 24.576 MHz | — | Stage 3 |
| BCLK frequency | (sample rate × slots × slot width) | — | Stage 3 |
| LRCK frequency | 48 kHz (or configured rate) | — | Stage 3 |
| Output full-scale level | TBD | — | Stage 4/5 |
| ADC input full-scale level | TBD | — | Stage 4/5 |
| Output THD+N at −1 dBFS | < 0.1% (target) | — | Stage 5 |

---

## Notes

All measurements should be recorded with board firmware version, MT32 firmware version, and test date.

---

## Related Documents

- [`docs/bringup-plan.md`](bringup-plan.md)
- [`docs/line-level-and-headroom.md`](line-level-and-headroom.md)
