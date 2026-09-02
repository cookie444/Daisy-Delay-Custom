# Build Guide — staged, test-as-you-go

Build this in stages. Each stage is independently testable, so when something
misbehaves you know it's the thing you just added — not "somewhere in 20
wires."

The big idea: **you already have working, proven delay code on the Pod.** So
get that same delay running on the Seed with *no controls at all* (fixed
values) before you wire a single knob. DSP first, hardware second.

---

## Stage 0 — Smoke test
**Parts:** Daisy Seed, USB-C cable. That's it.

Flash the Blink example exactly as you did when you first set up.

**Pass:** the onboard LED blinks.
**Proves:** the module is alive, your toolchain works, DFU works.

---

## Stage 1 — Audio passthrough
**Parts added:** 2 × TRS jack, hookup wire.
**Power:** USB-C. *Do not wire 9 V yet* — one less variable.

Wire in → `Audio In 1/2`, out → `Audio Out 1/2`, sleeve → AGND, and
**tie AGND to DGND**. Flash this:

```cpp
#include "daisy_seed.h"
using namespace daisy;
static DaisySeed hw;

void AudioCallback(AudioHandle::InterleavingInputBuffer  in,
                   AudioHandle::InterleavingOutputBuffer out,
                   size_t size)
{
    for(size_t i = 0; i < size; i++)
        out[i] = in[i];
}

int main(void)
{
    hw.Init();
    hw.StartAudio(AudioCallback);
    while(1) {}
}
```

**Test with a line-level source** — phone, MP3 player, or audio interface —
not your guitar (see the level warning below).

**Pass:** you hear the source cleanly through the output.
**Proves:** codec works, jacks wired right, grounds correct.

> **Level warning:** the Seed's codec is line level (~1 Vrms, ~13–30 kΩ input
> impedance). A guitar pickup wants ~1 MΩ and is much quieter, so straight in
> it will sound thin and weak. Testing with a line source right now avoids
> chasing a "problem" that's really just level. Stage 6 fixes it properly.

---

## Stage 2 — The delay, with no controls
**Parts added:** none.

Port your Pod delay to the Seed but **hard-code** time, feedback, and mix
(e.g. 400 ms / 0.45 / 0.5). Same DSP, new hardware wrapper: `DaisySeed`
instead of `DaisyPod`, `Interleaving` buffers, no `knob`/`button` objects.

Keep the delay internals identical to what you already validated — feedback
filter in the feedback path, `ReadHermite`, tap-tempo logic.

**Pass:** you hear repeats.
**Proves:** the delay DSP runs correctly on the Seed. This is the big one —
after this, everything left is just reading controls.

---

## Stage 3 — One knob, then the rest
**Parts added:** potentiometers (10 kΩ B linear).

Wire **one** pot first — K3 (MIX) on **D17**, using the pattern in
`pinmap.md`. Flash a test that reads it:

```cpp
AdcChannelConfig cfg[1];
cfg[0].InitSingle(hw.GetPin(17));
hw.adc.Init(cfg, 1);
hw.adc.Start();

// later, in your control loop:
float mix = hw.adc.GetFloat(0);   // 0.0 .. 1.0
```

**Pass:** turning the knob changes the wet/dry balance.
**Proves:** your pot wiring pattern is right.

Now **replicate that exact pattern** five more times — D15, D16, D18, D19,
D20. Because you proved the pattern on one pot, the rest is repetition, not
debugging.

> Buy all 6 pots up front; they're cheap, and stages 3+ go faster when you
> aren't waiting on parts.

---

## Stage 4 — Switches
**Parts added:** 3 mini toggles, 4-pos DIP, 2 footswitches.

All are the same simple pattern: one side to the GPIO, other side to GND.
Firmware enables the internal pull-up, so **closed = low**.

Test one first, then the rest:

```cpp
Switch sw;
sw.Init(hw.GetPin(9), 1000.f);   // pin, debounce update rate (Hz)

// in your control loop, call once per pass:
sw.Debounce();
if(sw.RisingEdge()) { /* pressed */ }
```

Wire FS1 (bypass, **D9**) and confirm you can toggle the effect, then add the
toggles (D2–D4), DIP (D5–D8), and FS2 (tap tempo, D10).

**Pass:** each switch does what the firmware expects.
**Proves:** GPIO + pull-ups.

---

## Stage 5 — LEDs
**Parts added:** 2 LEDs + 2 × 1 kΩ.

D11 (effect on) and D12 (tempo blink). Add these *before* boxing it so you
still have visual feedback on the bench.

---

## Stage 6 — Input buffer (do this before playing guitar through it)
**Parts added:** op-amp or JFET buffer.

A real guitar needs ~1 MΩ input impedance and some gain to reach line level.
The usual fix is a non-inverting op-amp buffer (e.g. TL072 / OPA2134) ahead of
the codec, or a single JFET follower. Add this once the rest works — it's the
difference between "thin and weak" and "sounds like a pedal."

Also worth adding on the output: a buffer/line driver so you can drive long
cables without loading the codec.

---

## Stage 7 — 9 V power
**Parts added:** 2.1 mm barrel jack, 1N5819, 100 µF, 100 nF, SMAJ15A.

Wire per `pinmap.md`. Run on 9 V with USB unplugged.

**Pass:** works on 9 V, **no new hum**. If hum appears, it's a grounding
issue — check that AGND/DGND are tied at exactly one point and the jack
sleeve isn't creating a loop.

---

## Stage 8 — Enclosure

Drill and mount: 6 knobs, 3 toggles, DIP, 2 footswitches, 2 LEDs, 2 jacks,
DC jack. Hammond 1590BB or similar compact box.

---

## Stage 9 — Firmware features

Now that the hardware is proven, add the fun stuff safely: tap subdivisions
(dotted / triplet / half-time from the DIP), freeze/hold, ping-pong, and the
tape-flavour toggle.

---

## Suggested buying order

| Order | Buy | Why |
|-------|-----|-----|
| 1 | Daisy Seed + USB-C | stages 0–2 need nothing else |
| 2 | 2 × TRS jack, wire | stage 1 |
| 3 | All 6 pots, 3 toggles, DIP, 2 footswitches, LEDs + resistors | stages 3–5 |
| 4 | Op-amp / JFET for buffer, resistors, caps | stage 6 |
| 5 | Barrel jack, 1N5819, caps, TVS | stage 7 |
| 6 | Enclosure, knobs, hardware | stage 8 |

Stages 0–2 cost you essentially nothing beyond the Seed — you can prove the
whole audio path and the delay DSP before committing to any parts.

## Debugging rules of thumb

- **Change one thing per stage.** If it worked before you touched it, it's
  what you just touched.
- **Suspect grounds first.** Most audio weirdness is grounding, not code.
- **Verify with a known-good source.** A phone/interface removes the guitar's
  level and impedance from the equation.
- **Measure, don't guess.** A multimeter on the pot wiper (should sweep
  0 → 3.3 V) settles most "the knob doesn't work" cases instantly.
