# BOM — Daisy Pedal Pod Delay (Seed v2 console)

Quantities per one pedal. Example part numbers are Mouser-friendly text.

## Core / Electronics

| # | Qty | Part | Spec / Value | Example part |
|---|----:|------|--------------|--------------|
| B1 | 1 | Daisy Seed v2 | STM32 H7 module | Electrosmith Daisy Seed |
| B2 | 1 | 5 V regulator | LDO 7805 (TO-92 or TO-220 ≥1A headroom) | LM7805CT |
| B3 | 1 | Reverse-prot. diode | Schottky 1N5819 (low VF) | 1N5819 |
| B4 | 2 | Electrolytic cap | 100 µF 25 V (input/output of LDO) | Nichicon UPW1E101MDD |
| B5 | 2 | Ceramic cap | 100 nF (LDO decoupling) | Kemet C315C104K5R5TA |
| B6 | 2 | LED | 3 mm, red & green (or RGB if tempo color) | |
| B7 | 2 | Current resistor | 1 kΩ 1/4 W for LEDs | Yageo CF1/4W102JTB |
| B8 | 1 | 2.1 mm barrel jack | Center neg, DC-002 | CUI PJ-036AH |
| B9 | 2 | TRS jack 6.35 mm | Stereo jack for IN & OUT | Cliff STY-09012 |
| B10 | 1 | Terminal block | Optional, for wire pickup | — |
| B11 | misc. | Wire, solder, perf | Jumper/bench wire | — |

## Controls

| # | Qty | Part | Spec | Example part (Alpha/Bourns) |
|---|----:|------|------|------------------------------|
| C1 | 6 | Potentiometer | 10 kΩ B linear, 9 mm | Alpha RK090 |
| C2 | 3 | Mini toggle | SPST/SPDT mini, solder-lug | E-Switch 100SP1T2B4M2QE |
| C3 | 1 | DIP switch | 4-position slide DIP, 2.54 mm | CTS 208-4 |
| C4 | 2 | Footswitch | Soft-touch momentary, non-latching | C&K AP-series / Davina CLK |
| C5 | 6 | Knobs | Top-hat/aluminum style | Davies 6.35mm |
| C6 | 1 | DIP pad (6-pos) | If the pack says 6, leave 2 open | — |

## Structure / Finish

| # | Qty | Part | Spec |
|---|----:|------|------|
| S1 | 1 | Enclosure | Compact box (Hammond 1590BB) |
| S2 | 4 | Feet/screws | Rubber feet, mounting hardware |
| S3 | 1 | PCB/perf-board | For Seed + pots + harness |
| S4 | 1 | Stickers/Nameplate | UV-printed labels if desired |

## Wiring note

Wire the 6 pots through a ribbon/Belden bundle; star AGND/DGND together at
the LDO to avoid hum. If you swap the 7805 for a buck regulator, keep the
diode + caps and confirm VIN accepts the rails.

### Source list (typical)

- Daisy Seed: electro-smith.com
- Pots/toggles/DIP: Mouser (Alpha, E-Switch, CTS)
- Footswitch: LoveMySwitches / C&K
- TRS jacks: Cliff STY series
- Enclosure: Hammond 1590BB
- Regulator: Any 5 V LDO ≥ 200 mA (7805 used for simplicity)
