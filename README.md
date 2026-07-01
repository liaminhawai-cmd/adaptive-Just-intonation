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
- **Color = consonance, not 12-TET.** Each note is colored by **Tenney harmonic distance**
  (`HD = log₂(n·d)` of the interval in lowest terms — octave≈1, fifth≈2.6, major third≈4.3,
  7/4≈4.8, tritone≈10). Lower = simpler ratio = more consonant = greener. Crucially the color
  is **relational and live**: a note is colored by its consonance *with the notes you're
  currently holding or have just played* (recently-played notes decay over a few seconds), so
  the whole lattice recolors around whatever you're playing. With nothing held it falls back to
  consonance vs. the root.

## Tune-by-dragging (the main way to play)

- **Right-click any node** → spawns a child tuned *from* that note.
- **Drag up/down** → the new pitch **locks to just intervals** from the parent.
- **Scroll wheel** sets the prime limit of the snap grid: all the way down = octaves only
  (2-limit), scroll up to add fifths (3), thirds (5), harmonic 7ths (7), then 11, 13. Simpler
  ratios have a wider capture zone, so they're stickier — "privilege simple ratios," built in.
- While dragging, **ratio lines fan out to the active notes**, each labeled with the interval
  and colored by its consonance — a live read of what you're making against what's sounding.
- **Click** to drop the note · **right-click background / Esc** to cancel.

### Lattice axes (5/7-limit)

- → right = ×3 (perfect fifths)
- ↑ up = ×5 (major thirds)
- ⟋ diagonal = ×7 (harmonic sevenths)
- Octaves (×2) are folded out of the layout but kept in the real frequency.

## Ben Johnston notation

Notes are labeled in Ben Johnston's just-intonation notation (nominals = the just
C-major scale, accidentals applied on top):

- `♯` / `♭` = chromatic semitone 25/24
- `+` / `−` = syntonic comma 81/80 (the 5-axis)
- `7` / `L` = septimal inflection 35/36 (the 7-axis; `L` is the inverted-7)
- `↑` / `↓` = undecimal inflection 33/32 (the 11-axis)

So `7/4` shows as **B♭7**, `11/8` as **F↑**, `81/64` as **E+**, `6/5` as **E♭**.
Ratios beyond the 13-limit fall back to showing the fraction.

## Sequencer (1-bar loop)

A 16-step, one-bar sequencer is docked at the bottom:

- **BPM** field + **Play/Stop** loop with a moving playhead.
- One lane per note (labeled with Johnston name + ratio, high pitch at top).
- **Click a cell** to toggle a hit; cells are tinted by that note's live consonance.
- **The tune-by-drag works here too**: right-click a lane (or cell) to set it as the
  source, move vertically to lock a new pitch to a just interval from it, scroll to set
  the prime limit, then click to drop the note at that step (it's added to the lattice too).

## The living map

The lattice is not static — it breathes with what you play:

- **Salience**: every note gains salience when played (clicks, chords, sequencer steps)
  and decays over ~7 seconds. Salience drives each node's **size and opacity**, so
  recently-used notes stay big and bright while neglected ones shrink and fade.
- **Follow** (toolbar toggle): the view gently drifts to keep the centre of recent
  activity in frame. Pauses while you pan or tune manually.
- **Hub** (toolbar toggle): rings the note with the smallest total ratio-distance
  (Tenney harmonic distance) to what's currently active — a *harmonic* hub.
  It is **not** a functional tonal centre / key: a ratio metric can't see voice-leading,
  meter, or cadence, so it may pick e.g. G where your ear hears C. Treat it as a hint.

## Navigation

- **Two-finger scroll = pan**, **pinch / ctrl+scroll = zoom**, plus on-screen `+ / − / Fit`.
- In tune/placement mode, **scroll sets the prime limit** instead (debounced for trackpads).

## What's here now

- Interactive lattice colored by live consonance, with Johnston labels
- Right-click drag-to-tune (lattice *and* sequencer) with scroll-set prime limit + ratio lines
- 1-bar 16-step sequencer with tempo, loop, and per-note lanes
- Add-note-by-ratio (typed) with live preview + comma-collision warning
- Presets: 7-note JI major, 5-note pentatonic, 7-limit (harmonic 7th)
- Per-note table: Johnston name, ratio, cents, Hz, harmonic distance (HD)
- In-browser playback (single note, chord of selected)

## Not here yet (on purpose)

- **Nothing is auto-corrected.** No adaptive drift fixing; you place every comma yourself.
- **No DAW/MIDI output yet.** The sequencer is for sketching; getting these tunings into
  FL Studio is the roadmap below.

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
