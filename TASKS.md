# Lattice — task list for follow-up sessions

Judgment calls are already made; these are well-specified builds. Test with
Playwright headless (`NODE_PATH=/opt/node22/lib/node_modules`, chromium at
`/opt/pw-browsers/chromium`), bump `BUILD`, push to `main` (auto-deploys).
Philosophy guardrails: everything is exact fractions; nothing auto-corrects
tuning; cents only behind the `showCents` toggle; new pitches always have a
parent + interval on the lattice.

## 1. Transpose-on-drag for selections (the big one) — Opus
Select ≥1 notes → Ctrl+drag (or a dedicated hotkey) moves the whole selection
vertically as a ghost. On drop, open a chooser (reuse the pivot/tunebox UI):
- **Preserve scale degrees**: each note maps to the pitch class N steps up or
  down within the CURRENT key signature (gamut order by cents). CMaj7 dragged
  up two degrees in a diatonic key becomes Emin7 — no new notes created.
- **Preserve intervals, tuned from an anchor**: the intervals between selected
  notes stay exact; pick which note anchors (default = the dragged-onto lane's
  pitch; offer chips for each selected note like the pump suggestions: "keep
  the G", "keep the B"…). Retune = multiply all by one exact factor (reuse
  `retuneBlock` + `nearestUnison`); create new pitch classes where needed
  (they get parent = anchor source, interval = the factor).
- Preview before/after like the pump box. Esc cancels the drop.

## 2. Score view v2 + composing in it — Opus
Current `renderScore` is read-only with naive x=time placement.
- Click on staff = add note at snapped time (use `curDur`, `timeSnap`); the
  vertical click position picks the nearest key-signature pitch class + octave
  (NOT chromatic — key signature is the gamut).
- Drag notehead vertically = same behavior as roll move-drag (retune to lane),
  horizontally = move in time. Right-click = delete.
- Render Johnston accidentals once per pitch class per bar (like real
  accidental conventions), not on every notehead.
- Rhythmic spacing: x proportional to time is fine, but add beat ticks and
  don't let noteheads overlap (min horizontal gap; offset clusters).
- Keep the roll as the source of truth; score edits mutate `hits`.

## 3. More demos — Sonnet
- Public-domain piano transcriptions (short, recognizable A-sections only):
  Satie Gymnopédie No.1, Debussy 1st Arabesque opening, Clair de Lune opening,
  Bach chorale phrase (chorales show JI triads best). Use the DEMOS/mel format
  or CHORD_DEMOS; keep each ≤ 8 bars. NO copyrighted material (nothing post-1929
  US PD line; when unsure, skip).
- Barbershop tag (the classic "hanger"): melody + 3 parts, 7-limit dom7 —
  barbershop is THE genre that locks into JI.
- Chromatic-mediant cycle (C→E→A♭ majors) as another diesis spiral.

## 4. Rel-line polish — Sonnet
- Arrowheads on relationship lines (marker-end SVG defs).
- Clamp labels into the visible grid viewport (they can currently sit above
  the top lane or beyond the right edge when partners are far away).
- Don't draw lines to partners scrolled far outside the viewport; draw a
  short stub + label at the edge instead.

## 5. Synth v3 — Sonnet
- Per-oscillator stereo pan (StereoPannerNode) + a "spread" macro.
- Unison: 2–5 voices per osc, detune expressed as a RATIO (e.g. 81/80 spread),
  philosophy-consistent.
- Glide (portamento) for the mono/drone path: exponentialRampTo on frequency.
- Simple reverb: generated impulse (2s decaying noise) into a ConvolverNode
  send with a mix slider.
- Per-note velocity: alt+drag vertically on a note bar sets 0–127; scale gain
  and MIDI velocity.

## 6. Loop/arrangement QoL — Sonnet
- Double-click loop rail = loop that pattern's full length; shift+drag on the
  rail = draw a new loop region (FL-style).
- Rename patterns (dblclick chip), reorder arrangement chips by drag.
- Per-entry repeat counts in the arrangement (A×4 B×2).
- Song-mode export: MIDI export should honor songMode (concatenate patterns).

## 7. Known bugs / debts
- Live edits while the SONG is playing don't reschedule (pattern loop does).
- Score view ignores the loop region overlay (drawLoop height guess).
- `hitAbsCents` linear scan per hit per frame in big songs — memoize if slow.
- MIDI import always lands in pattern A / wipes patterns (fine for now; maybe
  offer "import into new pattern").
- Undo of a pattern switch restores state but not the pattern-bar highlight
  until next render (cosmetic).

## Context for the next model
- `patterns[]`/`curPat`/`arrangement`/`songMode` + `packCurrent()`/`unpackPattern()`
  are the pattern core; `hits`, `BARS`, `loop` are the CURRENT pattern's live state.
- Key signature = per-pattern `gamut` (note-id list) applied to `n.gamut` flags.
- `retuneBlock(hits, factor)` + `nearestUnison(f)` are the retuning primitives.
- `timeSnap(xBars, excludeHit)` is additive (boundaries + divisions).
- The `tet12`/`outHz()` hook quantizes playback only — never the model.
