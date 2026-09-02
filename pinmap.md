# Daisy Seed v2 — Hardware Pinmap

Boutique delay pedal (Chase-Bliss-style compact box).

## How to read this

`GetPin(idx)` is the libDaisy accessor, and **the index equals the `D` number
printed on the Seed's header silkscreen** (e.g. `GetPin(16)` == header `D16`).
So the "Seed" column below is what you look for on the board.

These follow libDaisy's **default** `GetPin()` table (the `SEED_REV2` block is
*not* compiled in by default), which matches the current Daisy Seed datasheet.
If you ever compile with `SEED_REV2` defined, the mapping changes — don't.

## Pots (ADC inputs)

ADC-capable pins are fixed in hardware. From `lib/libDaisy/src/per/adc.cpp`,
the full set is: PA6, PC4, PB1, PA7, PC5, PB0, PC0, PC1, PC2, PC3, PA2, PA3,
PA0, PA1, PA4, PA5. The Seed exposes 12 of these as `D15`–`D25` and `D28`.

| Knob | Function | Seed | GetPin | STM32 | ADC ch |
|------|----------|------|--------|-------|--------|
| K1 | TIME      | D15 | 15 | PC0 | 10 |
| K2 | REPEAT    | D16 | 16 | PA3 | 15 |
| K3 | MIX       | D17 | 17 | PB1 | 5  |
| K4 | TONE      | D18 | 18 | PA7 | 7  |
| K5 | MOD DEPTH | D19 | 19 | PA6 | 3  |
| K6 | MOD RATE  | D20 | 20 | PC1 | 11 |

Spare ADC pins left over for expansion: D21, D22, D23, D24, D25, D28.

Wiring per pot: one outer lug → **+3V3A**, other outer lug → **AGND**, wiper →
the pin. Add 100 nF from wiper to AGND. Never feed pots from 9 V — the ADC is
0–3.3 V.

## Toggle switches (3)

| Toggle | Function | Seed | GetPin | STM32 |
|--------|----------|------|--------|-------|
| T1 | Echo flavour: DIGITAL / TAPE | D2 | 2 | PC10 |
| T2 | Ping-pong off/on            | D3 | 3 | PC9  |
| T3 | Modulation off/on           | D4 | 4 | PC8  |

## DIP switch (4-position, top-mount)

| DIP | Function | Seed | GetPin | STM32 |
|-----|----------|------|--------|-------|
| DIP1 | Tap subdiv: dotted   | D5 | 5 | PD2  |
| DIP2 | Tap subdiv: triplet  | D6 | 6 | PC12 |
| DIP3 | Tap subdiv: half-time| D7 | 7 | PG10 |
| DIP4 | Freeze / hold loop   | D8 | 8 | PG11 |

## Footswitches (soft-touch momentary, 2)

| Foot | Function | Seed | GetPin | STM32 |
|------|----------|------|--------|-------|
| FS1 | Bypass    | D9  | 9  | PB4 |
| FS2 | Tap tempo | D10 | 10 | PB5 |

## LEDs (2)

| LED | Function | Seed | GetPin | STM32 |
|-----|----------|------|--------|-------|
| LED1 | Effect on (green)            | D11 | 11 | PB8 |
| LED2 | Tempo blink, red/blue by mod | D12 | 12 | PB9 |

Each in series with a 1 kΩ resistor, cathode to GND. These deliberately use
non-ADC pins so the analog-capable ones stay free.

## Power

**No 5 V regulator is required.** VIN accepts **+5 V to +17 V** (older
datasheets: +4 V), so a standard 9 V pedal supply feeds the Seed directly —
its onboard regulators derive 3.3 V. A 7805 would only waste ~0.6 W as heat.

- 9 V center-negative barrel → **series** Schottky (1N5819, reverse
  protection) → **VIN**; sleeve → **GND**.
- **AGND must be tied to DGND** — required by the datasheet.
- Bulk 100 µF electrolytic + 100 nF ceramic across VIN/GND.
- Recommended: 15 V TVS (SMAJ15A) across the input. Some pedalboard supplies
  output 18 V, which exceeds the 17 V maximum.
- Add a regulator only if your supply can exceed 17 V.

## Audio

- IN:  jack tip → `Audio In 1`, ring → `Audio In 2`, sleeve → AGND
- OUT: jack tip ← `Audio Out 1`, ring ← `Audio Out 2`, sleeve → AGND

Use the official Daisy Seed pinout card to locate `Audio In 1/2`,
`Audio Out 1/2`, `VIN`, `AGND`, `DGND` and `+3V3A` on the header — they are
labelled on the silkscreen.

> **Level warning:** the Seed's codec is **line level** (~1 Vrms), not
> instrument level. A guitar straight in will be quiet and will load the
> pickup. See `BUILD_GUIDE.md` stage 6 for the input buffer.
