# Schematic — Daisy Pedal Pod Delay (Seed v2)

ASCII reference for hand-wiring/perf-board layout. Net names map 1:1 to
`hardware/pinmap.md`.

```
                 ┌──────────────────────────────┐
  9V DC ─────────│ 1N5819 (series) ──────── VIN │  barrel jack, 9V
                 │                              │
                  │            DAISY SEED V2      │ AGND tied to DGND
                 │        (header pins used)     │
                 │                               │
 TRS IN L/R ─────│ AUDIO_IN_L/R                  │
 TRS OUT L/R ────│ AUDIO_OUT_L/R                 │
                 │                               │
 ┌─ Pots x6 ─────│ D16..D25 -> ADC channels      │
 │ K1 TIME       │ 16 (PB0)                      │
 │ K2 REPEAT     │ 17 (PC1)                      │
 │ K3 MIX        │ 18 (PA3)                      │
 │ K4 TONE       │ 19 (PA1)                      │
 │ K5 MODDEPTH   │ 24 (PC5)                      │
 │ K6 MODRATE    │ 25 (PA4)                      │
 └───────────────┴───────────────────────────────┘
 ┌─ T1 (flavor) ─│ 2 (PC11)                      │
 ┌─ T2 (pingpon)─│ 3 (PC10)                      │
 └─ T3 (modul) ──│ 4 (PC9)                       │
 ┌─ DIP1 dotted ─│ 5 (PC8)                       │
 ┌─ DIP2 triplet─│ 6 (PD7)                       │
 ┌─ DIP3 half ───│ 7 (PC12)                      │
 └─ DIP4 freeze ─│ 8 (PG10)                      │
 ┌─ FS1 bypass ──│ 9 (PG11)                      │
 └─ FS2 tap ─────│ 12 (PB8)                      │
 ┌─ LED1 (red/green?)─ 14 (PB6) ── 1k ── LED ── GND
 └─ LED2 (tempo) ── 15 (PB7) ── 1k ── LED ── GND
```

## Power stage (detail)

```
 9V barrel jack (center-negative)
   tip (+9V) ──── 1N5819 (series, reverse prot.) ──┬──── VIN (39)
                                                   │
                                                   ├── 100 uF ──┐
                                                   ├── 100 nF ──┤
   sleeve (GND) ───────────────────────────────────┴────────────┴── GND (40)

 Optional: SMAJ15A TVS across VIN/GND (clamps 18 V pedalboard supplies).
 AGND must be tied to DGND.  No 5 V regulator — VIN accepts 5-17 V.
```

## Pot section (6)

```
Pot_n (B linear, 10k):
  OUT-L = AGND    OUT-R = +3V3A   WIPER → Seed GetPin(idx)
  (optional 100 nF wiper→AGND)
```

## Toggle / DIP / Footswitch

All SPST/SPDT run pin→GND. Firmware enables internal pull-ups, so reading
`low` = switch closed. Wire GPIO on the driven contact, GND return on other.

## LEDs

```
Seed GetPin(idx) ── 1kΩ ── LED anode ── LED cathode ── GND
(For one RGB LED, tie both LEDs’ cathodes and drive two resistors.)
```

## Audio path (TRS)

```
IN  TRS tip = L, ring = R → Seed AUDIO_IN_L/R, codec handles level
OUT TRS tip = L, ring = R ← Seed AUDIO_OUT_L/R directly
AGND common star at the Seed; chassis to AGND at the DC jack.
```

## What is NOT on the schematic

- Codec (on Seed): `AK4556` handles filtering; no external R/C needed.
- USB-C: kept free for DFU flashing. Per the datasheet it is safe to power
  the Seed from VIN and USB at the same time.
```

Netlist:
1. Power: J1_9V tip → D1(1N5819) series → VIN(39) [+ 100uF/100nF to GND,
   optional SMAJ15A clamp]. J1 sleeve → GND(40). Tie AGND to DGND.
2. Audio: TRS_IN.L→AUDIO_IN_L; TRS_IN.R→AUDIO_IN_R;
   AUDIO_OUT_L→TRS_OUT.L; AUDIO_OUT_R→TRS_OUT.R.
3. Pots K1..K6 wipers → PB0, PC1, PA3, PA1, PC5, PA4 (in that order).
4. T1, T2, T3 → PC11, PC10, PC9.
5. DIP1–4 → PC8, PD7, PC12, PG10.
6. FS1, FS2 → PG11, PB8.
7. LEDs via 1k → PB6, PB7.
```
