# Breadboard Cart — Daisy Delay Custom

A breadboard build is for **proving the circuit** before anything gets soldered
or drilled. So this cart drops all the panel-mount hardware (enclosure,
footswitches, 1/4" jacks, DC jack) and substitutes parts that plug straight in
or clip on.

## Strategy: how to avoid soldering anything

| Problem | Solution |
|---|---|
| Pots don't plug into a breadboard | Use **solder-lug pots** and clip on **alligator-to-male jumpers** — clip the pot lug, plug the male pin into the breadboard |
| Footswitches don't fit | Use **6×6 mm tact switches** on the breadboard for now; swap to soft-touch footswitches at the enclosure stage |
| 1/4" jacks are panel-mount | Use **1/4" jacks with solder lugs** (A-7227) and clip on **alligator leads** — real 1/4" sockets, no adaptor needed |
| Power | Just use **USB** while prototyping. Add the MB-102 module if you want 3.3 V / 5 V rails on the breadboard |
| Knobs | Still worth having — the pots take standard 6.35 mm knobs |

Also: get **two** breadboards. The Seed is wide and eats a lot of rows.

## Order required elsewhere (unchanged)

- **Daisy Seed v2** — electro-smith.com (Tayda doesn't stock it)

## Breadboard & wiring

| Qty | Part | SKU | Price | Link |
|----:|------|-----|-------|------|
| 2 | 830-point solderless breadboard | A-2372 | $2.49 | https://www.taydaelectronics.com/830-point-solder-less-plug-in-breadboard.html |
| 1 | Jumper wires M/M, pack of 65 | A-6024 | $0.99 | https://www.taydaelectronics.com/premium-jumper-wires-male-male-pack-of-65.html |
| 1 | Jumper wires M/F, pack of 40 | A-6153 | $0.79 | https://www.taydaelectronics.com/connectors-sockets/jumper-wire-dupont/premium-jumper-wires-male-female-100mm-pack-of-40.html |
| 3 | **Alligator clip → male jumper**, 10 lines each (30 total) | A-5498 | $1.50 ea | https://www.taydaelectronics.com/connectors-sockets/jumper-wire-dupont/alligator-clip-to-male-jumper-wire-10-lines-awh24-200mm.html |
| 1 | MB-102 breadboard power module (3.3 V / 5 V rails) — optional | A-7404 | $0.75 | https://www.taydaelectronics.com/mb-102-breadboard-power-module-white-color.html |

The alligator leads are the key item — they let you hook pots and switches to
the breadboard with no soldering at all.

## Controls (breadboard stand-ins)

| Qty | Part | SKU | Price | Link |
|----:|------|-----|-------|------|
| 6 | B10K pot, 16 mm, **solder lugs**, 6.35 mm shaft | A-2981 | $0.50 | https://www.taydaelectronics.com/b10k-ohm-linear-taper-potentiometer-round-shaft-solder-lugs-l.html |
| 6 | Knob Davies 1900H clone, 6.35 mm | A-4559 | $0.42 | https://www.taydaelectronics.com/knob-davies-1900h-clone-black.html |
| 1 | 4-position DIP switch (fits breadboard) | A-6506 | $0.35 | https://www.taydaelectronics.com/electromechanical/switches-key-pad/dip-switch/black-dip-switch-4-positions-gold-plated-contacts-top-actuated.html |
| 4 | **Tact switch 6×6 mm** (bypass + tap tempo stand-ins) | A-5147 | $0.04 | https://www.taydaelectronics.com/tact-switch-6x6mm-11mm-through-hole-spst-no.html |

For the 3 mini toggles, either clip on the real SPDT toggles with alligators
(A-6755 from the main cart) or just use jumper wires to tie the pins high/low
while testing.

## Audio I/O

| Qty | Part | SKU | Price | Link |
|----:|------|-----|-------|------|
| 2 | **6.35 mm (1/4") stereo jack, unswitched, solder lugs** — REAN NYS212 | A-7227 | $0.99 | https://www.taydaelectronics.com/6-35mm-1-4-stereo-insulated-socket-jack-solder-lug.html |

These are genuine 1/4" **sockets**, so guitar cables plug straight in — no
adaptor. Clip alligator leads onto the three lugs (tip / ring / sleeve) and into
the breadboard. Get the **unswitched** version (NYS212): switched jacks have
normalling contacts that lift when a plug is inserted, which is one more thing
to get wrong while prototyping.

### If you'd rather use 3.5 mm jacks

The breadboard-friendly 3.5 mm socket is **A-853**
(https://www.taydaelectronics.com/3-5mm-stereo-enclosed-socket-chassis-jack.html).
To plug a 1/4" cable into one you need the adaptor pointing the **other** way —
**3.5 mm plug → 6.35 mm socket**:

- **A-7709**, $0.60 — https://www.taydaelectronics.com/3-5mm-male-to-6-35mm-female-stereo-audio-jack-adaptor-gold-plated.html
- **A-6513**, $0.80 — https://www.taydaelectronics.com/6-35mm-female-to-3-5mm-male-stereo-audio-jack-adaptor-gold-plated.html

Note A-5541 in the earlier revision was **backwards** (it is a 3.5 mm *socket*
on a 1/4" *plug*), and A-3663 is the same idea but out of stock.

## Passives & indicators (same as the main cart)

| Qty | Part | SKU | Price | Link |
|----:|------|-----|-------|------|
| 2 | Bi-color red/green LED, 3 mm | A-1076 | $0.04 | https://www.taydaelectronics.com/bi-color-led-3mm-red-green.html |
| 10 | 1 kΩ 1/4 W 1% metal film | A-2200 | $0.015 ea | https://www.taydaelectronics.com/10-x-resistor-1k-ohm-1-4w-1-metal-film-pkg-of-10.html |
| 10 | 0.1 µF 50 V ceramic disc | A-4008 | $0.01 ea | https://www.taydaelectronics.com/10-x-0-1uf-50v-ceramic-disc-capacitor-pkg-of-10.html |
| 1 | 100 µF 25 V electrolytic | A-4541 | $0.03 | https://www.taydaelectronics.com/100uf-25v-105c-radial-electrolytic-capacitor-6x11mm.html |
| 1 | 1N5819 Schottky | A-484 | $0.05 | https://www.taydaelectronics.com/1n5819-schottky-barrier-diode-1a-40v.html |

## Quick Order

Upload **`BREADBOARD_QUICKORDER.csv`** at
https://www.taydaelectronics.com/quick-order/ , or paste this (comma-separated):

```
A-2372,2
A-6024,1
A-6153,1
A-5498,3
A-7404,1
A-2981,6
A-4559,6
A-6506,1
A-5147,4
A-7227,2
A-1076,2
A-2200,10
A-4008,10
A-4541,1
A-484,1
```

That's 30 alligator leads: 18 for the six pots (3 lugs each), 6 for the two
jacks, and a few spare for the toggles.

## Not needed yet

Skip these until you're ready to build the real pedal: enclosure (A-5158),
soft-touch footswitches (A-1091), 1/4" panel jacks (A-5238), DC jack (A-5245),
proto board (A-5465), hook-up wire.

## Reminder

The codec is **line level**. On the breadboard, feed it from a phone or
interface first — a guitar straight in will sound thin until you add the input
buffer (build stage 6).
