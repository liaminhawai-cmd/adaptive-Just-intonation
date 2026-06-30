# Lattice — a ratio-based Just Intonation instrument

An experiment in **manual** extended Just Intonation, in the spirit of Ben Johnston —
but instead of his accidental notation, you build pitches by **choosing exact ratios
from any note to any other note**. Every note is an exact fraction relative to a parent
*you* pick, so you own every comma and every bit of drift. Nothing is auto-corrected.

This is the opposite philosophy to Hermode Tuning / deLaubenfels adaptive tuning (which
quietly fix drift *for* you). Here the drift is yours to place on purpose.

## Run it

Open `index.html` in any modern browser (Chrome/Firefox/Safari). No build, no install.
Click **▶ Play** buttons or any node to hear it (Web Audio).

## The idea

- **Every pitch is an exact ratio** (a fraction) relative to a root you set (e.g. C = 261.63 Hz).
- **To add a note you nominate a parent and a ratio**: "tune a new note a `5/4` above the E."
  The app computes the exact frequency and places it on the tonality lattice.
- **The comma becomes a visible choice.** A "D" reached as `9/8` above C and a "D" reached
  as `10/9` below E are two different dots, two different frequencies. The preview warns you
  when a new note is a near-collision with an existing one ("a *different* D, 21.5¢ apart").
- **Color = drift** from the nearest 12-TET piano key. That coloring *is* the comma, made visible.

### Lattice axes (5/7-limit)

- → right = ×3 (perfect fifths)
- ↑ up = ×5 (major thirds)
- ⟋ diagonal = ×7 (harmonic sevenths)
- Octaves (×2) are folded out of the layout but kept in the real frequency.

## What's here now

- Interactive lattice (pan / zoom / click-to-play)
- Add-note-by-ratio with live preview + comma-collision warning
- Presets: 7-note JI major, 5-note pentatonic, 7-limit (harmonic 7th)
- Per-note table: ratio, cents, Hz, signed drift from 12-TET
- In-browser playback (single note, chord of selected, ascending sweep)

## Roadmap — getting it into FL Studio

The prototype is the "brain." To actually play synths from it:

1. **MPE / per-channel pitch-bend out** — universal: works with any synth in FL by spreading
   notes across MIDI channels and bending each to its ratio. Crude but works everywhere.
2. **MTS-ESP master** — the clean path. ODDSound open-sourced the MTS-ESP library; building a
   small master plugin lets MTS-ESP-aware synths (Surge XT, u-he, etc.) read our exact tuning
   in real time. FL itself has no native microtuning/SysEx, but these run inside it as VSTs.
3. **Export** — Scala `.scl`/`.kbm` and `.tun` files for static snapshots of a scale.

(Those steps are native/plugin code you'd build and run on your own machine; this repo is the
engine + UI that drives them.)

## Why not just use Infinitone DMT / MTS-ESP?

Those are great for real-time pitch *selection* and adaptive auto-correction, but their model is
"scales/subsets you switch between." None centers on *"relate any note to any other note as a
ratio chain, and show me the comma I just created."* That specific interaction is the point here.
