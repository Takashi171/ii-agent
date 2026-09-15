# Roast & Reveal

A personalized party game — single self-contained HTML file, no build step, no dependencies. Open the HTML file directly in a browser to play.

## Files

- `roast-and-reveal.html` — the game itself, single-device only (players pass one phone/laptop around). This is the version safe to publish as a Claude Artifact.
- `roast-and-reveal-multiplayer.html` — identical game, plus an opt-in **Connect Phones** mode: a QR code lets each player join on their own phone and privately type their answers for Guess Who Said It and Hot Seat, instead of passing one device around. Requires Firebase and a real static host (see below) — it will **not** work as a Claude Artifact, since the artifact sandbox's CSP blocks the network calls Firestore needs.
- `roast-reveal-library.html` — a searchable reference page listing every prompt/dare/question in the game's content pools, grouped by section, with a collapsible section nav.

## How to play

Add 2+ players, pick a mode from the hub (grouped by category: Warm-Up, Roast & Vote, Secrets & Reveals, Whisper Games, Classics, Speed Round, For Two), and follow the on-screen prompts. See the in-app "How to Play" section for full rules per mode.

## Setting up Connect Phones (multiplayer)

`roast-and-reveal-multiplayer.html` needs two one-time setup steps before the QR/phone feature will work. If you skip these, the toggle just falls back to pass-the-phone mode automatically — nothing breaks.

### 1. Host the file somewhere with a real URL

Pick one:

- **Netlify Drop** (easiest, no account needed): go to https://app.netlify.com/drop and drag `roast-and-reveal-multiplayer.html` onto the page. You get a live URL instantly.
- **GitHub Pages**: enable Pages for this repo (Settings → Pages → choose a branch/folder that includes `roast-and-reveal/`), then visit `https://<your-username>.github.io/<repo>/roast-and-reveal/roast-and-reveal-multiplayer.html`.

### 2. Turn on Firestore in the Firebase project

The file already has the project's Firebase config wired in (project `roastandreveal-466e4`). In the [Firebase console](https://console.firebase.google.com/):

1. **Create a Firestore database** if you haven't yet (Build → Firestore Database → Create database; any region is fine).
2. **Set security rules** so the game can read/write session data (Firestore → Rules):
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /sessions/{sessionId} {
         allow read, write: if true;
       }
     }
   }
   ```
   This keeps things open to anyone with a room code — fine for a party game with no sensitive data, since rooms are short-lived and codes are only ever shared with the people you invite.
3. **(Optional but recommended) Set a TTL policy** so old rooms clean themselves up: Firestore → TTL → create a policy on collection `sessions`, field `expiresAt`. The app already writes that field (24 hours out) on every new room.

Once both are done, toggle "🔗 Connect Everyone's Phones" on the setup screen, start the party, and share the QR code or room code that appears.
