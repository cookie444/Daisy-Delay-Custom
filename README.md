# Daisy Delay Custom

A boutique-style analog-flavored delay pedal built around the Electrosmith **Daisy Seed v2**, with a Chase-Bliss-inspired control surface: **6 knobs, 3 mini toggles, a 4-position DIP, and 2 soft-touch footswitches**, TRS jacks, and standard 9 V pedalboard power.

## Status

| Area | State |
|---|---|
| Delay DSP | Proven and working on a Daisy Pod (bypass, tap tempo, modulation) |
| Hardware design | Complete: pinmap, schematic, BOM, staged build guide |
| Physical build | Not started — design only |

The delay DSP is developed on the Daisy Pod (sold commercially, zero soldering) so the sound and all firmware features are proven before the custom hardware is assembled.

## Controls

| Control | Function | Type |
|---|---|---|
| K1 TIME | Delay time | 100 kΩ pot |
| K2 REPEAT | Feedback (self-oscillates at max) | 100 kΩ pot |
| K3 MIX | Wet/dry mix | 100 kΩ pot |
| K4 TONE | Feedback-path brightness | 100 kΩ pot |
| K5 MOD DEPTH | Tape-style wobble depth | 100 kΩ pot |
| K6 MOD RATE | Wobble speed | 100 kΩ pot |
| T1 / T2 / T3 | Flavour / ping-pong / modulation | SPDT mini toggle |
| DIP 1–4 | Dotted / triplet / half-time / freeze | 4-position DIP switch |
| FS1 / FS2 | Bypass / tap tempo | Soft-touch footswitch |
| LED1 / LED2 | Status LEDs | Dual-color |

## Sound design

Analog-Signal delay in the style of a BBD tape echo: a lowpass filter sits in the **feedback path only**, so the first repeat is full-bandwidth and later repeats darken naturally — warm echoes without a permanent muffled wet signal.

## Repository map

| File | Contents |
|---|---|
| `pinmap.md` | Complete control → Seed-pin mapping, verified against the datasheet and libDaisy |
| `schematic.md` | ASCII schematic and netlist |
| `BOM.md` / `BOM.csv` | Parts list with example part numbers |
| `BUILD_GUIDE.md` | 10-stage build order, every stage independently testable |
| `README.md` | This overview |

## Key design decisions

- **No 5 V regulator.** The Seed's VIN accepts +5…+17 V, so a standard 9 V pedal supply feeds it directly. An 18 V-rated TVS (in series diode) protects against 18 V pedalboard supplies. This avoids the heat/whine of a linear regulator.
- **AGND tied to DGND** near the Seed (a hard datasheet requirement, done once to avoid ground hum).
- **Line-level codec.** The Seed's audio codec expects ~1 Vrms, so a guitar needs an input buffer (build stage 6) — the design accounts for this.
- **Analog pins budgeted.** The Seed exposes only a handful of ADC-capable pins; every control got a verified assignment that leaves spares.
- **Pin map verified against libDaisy defaults.** The legacy `SEED_REV2` pin table does not match the compiled hardware map; all assignments follow the actual defaults.

## Build plan (one stage tested at a time)

0. Blink
1. Audio passthrough
2. Delay with hard-coded parameters
3. Wire the knobs
4. Switches (toggles, DIP, footswitches)
5. LEDs
6. Input buffer (guitar level)
7. 9 V power supply
8. Enclosure work
9. Firmware feature polish

The guiding rule: the Seed and audio path are proven (stages 0–2) before any control wiring, so DSP and hardware are never debugged at the same time. USB power is used through stage 7.

## Firmware

The delay firmware targets the Daisy Pod today. Once the custom build passes stages 0–2 the exactly-same code moves to the Seed with only the hardware driver swapped — libDaisy presents the same API for both.