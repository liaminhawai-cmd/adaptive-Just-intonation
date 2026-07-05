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

## Sequencer

A multi-bar sequencer is docked at the bottom (drag its top edge to resize):

- **BPM**, **bars** (1/2/4/8), zoom, **Play/Stop** with a smooth playhead
  (Web-Audio-clock scheduling, exact rational event times).
- **Lanes = every pitch class in the key signature × every octave** (C2–C6 region);
  labels show Johnston name + octave, high at top.
- **Click a lane, drag right** = add a note and set its length. **Right-click a bar** deletes it.
- **Tune-by-drag works here too**: right-click a lane to set it as the source, move
  vertically to lock a new pitch to a just interval from it, scroll to set the prime
  limit, click to drop (it's added to the lattice and the key too).
- **Score ⇄ Roll toggle**: read-only grand-staff view with Johnston accidentals.

### Key signature (managing comma pumps)

The **♪ column** in the notes table is the current key signature: only ♪-checked pitch
classes get sequencer lanes. Comma-pump freely on the lattice — the grid stays at ~7
notes. `key = selected` / `key = all` buttons re-key as the piece moves. Newly tuned
notes join automatically (that's how you "add an accidental").

### Rhythm map (rational time)

Rhythm gets the same treatment as pitch. **𝅘𝅥𝅮 Rhythm** opens a lattice of divisions of
the bar — the 2-axis explicit (2/1 … 1/64, every halving its own node), with **×3 ×5 ×7
toggles** for triplet/quintuplet/septuplet space. Click divisions to enable them; note
starts and lengths **snap to enabled divisions**, simpler divisions stickier, layered
gridlines show each. Time is stored as exact fractions of the bar — 1/3, 3/20, wherever
you land.

## Commas, drones, and saving

- **Comma links**: any two pitch classes within ~35¢ get a dashed amber link on the
  lattice labeled with the exact comma (`81/80 · 21.5¢`). The drift you placed, named.
- **Drones**: **shift-click** a lattice node or sequencer lane to hold it (Esc releases
  all). Held notes stay salient, so the whole lattice recolours relative to the drone —
  tune by ear against it. The voice is harmonic-rich on purpose: beats are audible.
- **Autosave**: the whole project (notes, key, hits, rhythm map, tempo) persists in the
  browser. **Export/Import .json** in the sidebar to keep or share pieces.

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

## MIDI output → FL Studio (MPE-style, works now)

The sidebar has a **MIDI Output** panel. It uses the Web MIDI API (Chrome/Edge only — Safari and
Firefox don't support it) to send real MIDI, pitch-bending each note to its *exact* ratio.

There are two **modes**:

- **Mono** (default) — everything goes out on **channel 1**; before each note the channel is
  pitch-bent to the ratio. This works with **any** synth (Harmor, FL Keys, Serum, …) because
  normal synths apply one global pitch bend. It's correct for **one note at a time** (melodies);
  in a chord, all held notes share the most recent note's bend. This is the mode to use with FL.
- **MPE** — notes spread round-robin across channels 2–16, each channel bent separately. Only
  correct if the receiving synth is genuinely in **MPE mode** (per-channel bend). FL's stock synths
  are *not* MPE, so they'll play the right notes but ignore the per-channel bends (→ 12-TET). Use
  this only with a real MPE synth.

**Setup:**
1. Create a virtual MIDI cable so the browser can "wire into" FL:
   - **Windows**: install [loopMIDI](https://www.tobias-erichsen.de/software/loopmidi.html), make a port (e.g. "Lattice").
   - **Mac**: enable the built-in **IAC Driver** (Audio MIDI Setup → MIDI Studio → IAC Driver → check "Device is online").
2. In this app: **Enable MIDI** → pick that virtual port from **Output device** → leave **Mode** on **Mono**.
3. In FL Studio: set the instrument track's MIDI input to that same virtual port, omni/all channels.
4. On the synth itself, make sure its **pitch-bend range is set to 2 semitones** (the default for most synths).
   Match the **Pitch-bend range** field in this app's MIDI panel if your synth uses a different default.
5. Play/click notes or run the sequencer here — FL receives real MIDI notes, each bent to the exact ratio.

Caveats: Mono mode is monophonic-correct only — for true polyphonic microtonal chords you need a
real MPE synth (MPE mode) or the MTS-ESP path below. Fine for melodic sketching as-is.

## Roadmap — a cleaner path later

1. **MTS-ESP master** — the tidy alternative to per-channel MPE bending. ODDSound open-sourced the
   MTS-ESP library; a small master plugin would let MTS-ESP-aware synths (Surge XT, u-he, etc.)
   read our exact tuning table directly, with full velocity/expression intact. FL itself has no
   native microtuning/SysEx, but these run inside it as VSTs. This is native/plugin code you'd
   build separately — this repo is the engine + UI that would drive it.
2. **Export** — Scala `.scl`/`.kbm` and `.tun` files for static snapshots of a scale.

## Why not just use Infinitone DMT / MTS-ESP?

Those are great for real-time pitch *selection* and adaptive auto-correction, but their model is
"scales/subsets you switch between." None centers on *"relate any note to any other note as a
ratio chain, and show me the comma I just created."* That specific interaction is the point here.
