# Roast & Reveal

A personalized party game — single self-contained HTML file, no build step, no dependencies. Open `roast-and-reveal.html` directly in a browser to play.

## Files

- `roast-and-reveal.html` — the game itself. Pass-the-phone/pass-the-device party game with 13 modes (Most Likely To, Icebreakers, Guess Who Said It, Truth or Dare, Charades, Truth Bottle, Paranoia, Vote, Hot Seat, 5 Second Rule, Pass the Bomb, Partners, Friends), depth/difficulty tiers, an optional After Dark content layer, and a "how well do you know each other" pair-scoring stat.
- `roast-reveal-library.html` — a searchable reference page listing every prompt/dare/question in the game's content pools, grouped by section, with a collapsible section nav.

## Status

Currently single-device only (players pass one phone/laptop around). A QR-code multi-device flow (so each player can privately type answers on their own phone for modes like Guess Who Said It and Hot Seat) was scoped out using Firebase/Firestore, but requires hosting the game on a static host (e.g. GitHub Pages or Netlify) rather than as a Claude Artifact, since the artifact sandbox's CSP blocks the outbound network calls Firestore needs. That hosting migration + Firebase/QR integration is not yet implemented.

## How to play

Open `roast-and-reveal.html`, add 2+ players, pick a mode from the hub (grouped by category: Warm-Up, Roast & Vote, Secrets & Reveals, Whisper Games, Classics, Speed Round, For Two), and follow the on-screen prompts. See the in-app "How to Play" section for full rules per mode.
