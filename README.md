# 🌺 Ukulele Noodle

A browser-based ukulele app for **exploratory noodling** — pick up a simple shape, hear it,
mess around, and stumble into happy accidents. No lessons, no streaks, just a calm canvas for play.

**[▶ Play it live](https://wkathomson.github.io/ukulele-noodle/)**

## Features
- **Clickable fretboard** — gCEA standard, reentrant (high-G) *or* low-G, correct in both note and bass logic.
- **Synthesised tone** — plucked-string (Karplus-Strong) audio via the Web Audio API. No samples, fully self-contained.
- **Strum & pick** — down/up strum with natural stagger, plus arpeggiated pick.
- **Chord browser** — easy one- and two-finger grips plus cheat voicings, tagged by finger count.
- **Reverse lookup** — play freely and it names the chord (or owns up to an "unnamed voicing").
- **Noodle extras** — slide a shape up/down the neck, suggested neighbouring chords, random shape, drone pad, strum patterns.

## Tech
Single `index.html` — vanilla JS, no build step, no framework. British English throughout, mobile-first.
The only external asset is the decorative *Pacifico* web font, which falls back to a system cursive offline.

Made with aloha 🌺
