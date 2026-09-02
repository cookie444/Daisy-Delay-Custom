# Daisy Seed v2 — Hardware Pinmap

Boutique delay pedal (Chase-Bliss-style compact box). All GPIO references
are libDaisy `GetPin(index)` indices on the Daisy Seed v2 (REV2 mapping,
verified against `lib/libDaisy/src/daisy_seed.cpp`).

## Pots (ADC inputs)

| Knob | Function | Seed index | STM32 pin | ADC ch |
|------|----------|-----------|-----------|--------|
| K1   | TIME     | 16 | PB0 | IN14 |
| K2   | REPEAT (feedback) | 17 | PC1 | IN9 |
| K3   | MIX      | 18 | PA3 | IN3 |
| K4   | TONE (repeats brightness) | 19 | PA1 | IN17 |
| K5   | MOD DEPTH | 24 | PC5 | IN13 |
| K6   | MOD RATE  | 25 | PA4 | IN4 |

Note: PA6/ch 6 is **excluded from libDaisy's ADC channel table**, so it is
avoided on purpose.

Each pot: one outer lug to +3V3A, other outer lug to AGND, wiper straight
into the pin. Add 100 nF decoupling from wiper to AGND (recommended).

## Toggle switches (3)

| Toggle | Function | Seed index | STM32 pin |
|--------|----------|-----------|-----------|
| T1 | Echo flavor: DIGITAL / TAPE | 2 | PC11 |
| T2 | Ping-pong: off/on | 3 | PC10 |
| T3 | Modulation: off/on | 4 | PC9 |

SPST/SPDT minis wired pin→GND; firmware uses internal pull-ups.

## DIP switch (4-position, top-mount)

| DIP | Function | Seed index | STM32 pin |
|-----|----------|-----------|-----------|
| DIP1 | Tap subdiv: dotted | 5 | PC8 |
| DIP2 | Tap subdiv: triplet | 6 | PD7 |
| DIP3 | Tap subdiv: half-time (x2) | 7 | PC12 |
| DIP4 | Freeze / hold loop | 8 | PG10 |

## Footswitches (soft-touch momentary, 2)

| Foot | Function | Seed index | STM32 pin |
|------|----------|-----------|-----------|
| FS1  | Bypass | 9 | PG11 |
| FS2  | Tap tempo | 12 | PB8 |

## LEDs (2)

| LED | Function | Seed index | STM32 pin |
|-----|----------|-----------|-----------|
| LED1 | Effect on (green) | 14 | PB6 |
| LED2 | Tempo blink, red/blue by mod (PWM-capable pin) | 15 | PB7 |

Wire through series resistors (1 kΩ) to LED anode, cathode to GND.

## Power

**No 5 V regulator is required.** VIN accepts **+5 V to +17 V** (older
datasheets: +4 V), so a standard 9 V pedal supply feeds the Seed directly —
the Seed's own onboard regulators derive 3.3 V. Feeding it through a 7805
would only waste ~0.6 W as heat.

- 9 V center-negative barrel → **series** Schottky (1N5819, reverse
  protection) → **VIN (pin 39)**; sleeve → **GND (pin 40)**.
- **AGND must be tied to DGND** — required by the datasheet.
- Bulk 100 µF electrolytic + 100 nF ceramic across VIN/GND.
- Recommended: 15 V TVS (SMAJ15A) or Zener clamp across the input. Some
  pedalboard supplies output 18 V, which exceeds the 17 V maximum.
- Add a regulator only if your supply can exceed 17 V.

Pots and LEDs reference **+3V3 Analog**, never 9 V — ADC/GPIO is 0–3.3 V.

## Audio

TRS in/out wired directly to the Seed's codec path nets
(`AUDIO_IN_L/R`, `AUDIO_OUT_L/R`); AGND/DGND join at the Seed.
One TRS jack carries stereo, so use one matching jack per direction.
