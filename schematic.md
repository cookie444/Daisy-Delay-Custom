# Schematic — Daisy Pedal Pod Delay (Seed v2)

ASCII reference for hand-wiring/perf-board layout. Net names map 1:1 to
`hardware/pinmap.md`.

```
                 ┌──────────────────────────────┐
  9V DC jack ────│ 1N5819 rev-prot ── 7805 ─VIN │  Drive jack +139
                 │                              │
                 │            DAISY SEED V2      │ AGND joined @ LDO
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
                ╔════════════╗  ╔═════╗   ╔═════════╗
 9V jack –━━ ┈─ │ J1 │ 2.1mm ── 1N5819  ──── 7805    ├── +5V ── VIN(39)
                ║    ║          must go to GND with 100uF/100n both sides
                ╚════╝ −├ GND ●── AGND/DGND ●── Jack tip return (ring)
```

## Pot section (6)

```
Pot_n (B linear, 10k):
  OUT-L = GND     OUT-R = 3V3     WIPER → Seed GetPin(idx)
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
- USB-C: kept free for DF flashing, isolated from the 5V rail by the Seed.
```

Netlist:
1. Power: J1_9V → D1(1N5819)A → U1(7805)in → +5V to VIN. J1 GND → GND star.
2. Audio: TRS_IN.L→AUDIO_IN_L; TRS_IN.R→AUDIO_IN_R;
   AUDIO_OUT_L→TRS_OUT.L; AUDIO_OUT_R→TRS_OUT.R.
3. Pots K1..K6 wipers → PB0, PC1, PA3, PA1, PC5, PA4 (in that order).
4. T1, T2, T3 → PC11, PC10, PC9.
5. DIP1–4 → PC8, PD7, PC12, PG10.
6. FS1, FS2 → PG11, PB8.
7. LEDs via 1k → PB6, PB7.
```
