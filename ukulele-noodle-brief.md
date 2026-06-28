# Claude Code brief: Ukulele noodling app

## What we're building

A browser-based ukulele app focused on **exploratory noodling**, not structured learning. The point is to pick up a simple chord shape, hear it, mess around, and stumble into happy accidents. No lessons, no syllabus, no progress tracking, no streaks. A canvas for play.

Standard tuning is gCEA (reentrant). The reentrant high-G string is core to the instrument's character and the note logic must handle it correctly rather than treating it as a low G.

This is a **separate app from the fretboard app** – a clean single-file build, reusing the fretboard's patterns where sensible but not retrofitting its string/tuning logic.

## Stack and constraints

- Single HTML file for markup, CSS and JS, deployable to GitHub Pages, plus a small folder of audio sample files loaded at runtime (see audio note below).
- Vanilla JS, no build step. No framework.
- Web Audio API for sound, using **sampled ukulele**. Load a small set of plucked-note samples (a few across the range, pitch-shifted to cover the rest) from a sibling folder on GitHub Pages, decoded into AudioBuffers and triggered via Web Audio. This breaks the strict single-file model but keeps deployment trivial. Alternative if a true single file is wanted later: base64-embed the samples into the HTML – flag this but default to the folder approach.
- British English throughout in UI copy.
- Mobile-first: must work well on a phone held in portrait, since "noodling on the train" is a real use case. Touch targets sized accordingly.
- No external dependencies beyond what's strictly needed; prefer zero.

## Core features (build these first)

### 1. Clickable fretboard
- Ukulele neck, four strings, ~12 frets. gCEA standard.
- A toggle for reentrant vs linear (low-G) tuning that correctly changes note and chord logic.
- Tapping a fret sounds the note and shows what's being built.
- Clear visual state for currently held notes.

### 2. Chord browser
- Shows a named shape on the neck.
- Filtered to genuinely easy grips: one- and two-finger chords (C, Am, F, G7, etc.), plus "cheat" voicings that sound good with minimal effort.
- Shapes tagged by finger count and difficulty so the user can deliberately stay in the shallow end.

### 3. Free noodle / reverse lookup
- Place fingers freely anywhere on the neck; the app names the chord or voicing you've landed on.
- This is the heart of the app: "what did I just play, and is it a real chord?"
- Handle partial/ambiguous shapes gracefully (suggest nearest named chord, or say it's an unnamed voicing).

### 4. Audio playback
- Strum and pick playback for any shape.
- Sampled ukulele tone, pitch-shifted from a small sample set. Per-note triggering so strum has a slight stagger.

## Complementary features (build after core works)

- **Shape shifting** – take any shape and slide it up/down the neck, hearing how the same grip becomes a different chord. Surfaces movable voicings.
- **Chords that go together** – pick a chord, surface 3–4 that sit nicely next to it, so noodling drifts into mini-progressions without becoming a theory lesson.
- **Random shape generator** – throw a constrained-random easy shape at the user to break habits and force fresh fingerings.
- **Drone / loop pad** – hold a root note or simple loop underneath so the user can noodle melodies and shapes over a tonal centre.
- **Strum-pattern toggle** – a couple of basic rhythmic patterns so a static shape becomes something musical to play along with.

## Design direction

- Calm, tactile, playful – this is a toy, not a tool. Avoid anything that reads as a "course".
- The fretboard is the hero element; everything else is secondary controls around it.
- Immediate feedback on every interaction (sound + visual) with no latency that breaks the noodling flow.
- One main view; modes (browse / noodle) switch in place rather than navigating to separate screens.

## Build order

1. Fretboard rendering + tuning model (reentrant correct) + tap-to-sound.
2. Sampled-ukulele audio (load + decode + pitch-shift) + strum/pick playback.
3. Chord browser with the easy-shapes library and difficulty tags.
4. Reverse lookup / free noodle mode.
5. Shape shifting.
6. Chords-that-go-together.
7. Random shape generator, drone pad, strum patterns.

Get 1–4 solid and pleasant before touching 5+.

## Chord detection approach (decided: hybrid)

Chord/voicing detection is the hardest part and drives the reverse-lookup feature. Use a **hybrid**:

- A curated lookup table of named easy shapes is checked first – simple, predictable, and matches the "easy shapes" framing of the chord browser.
- For anything not in the table (free noodled voicings), fall back to computing the chord name from the intervals of the held notes.
- When the interval logic can't name something cleanly, say so plainly ("unnamed voicing") rather than forcing a label, and optionally suggest the nearest named shape.

This keeps the common cases tight and named while still handling whatever the user stumbles into.
