# 🐍 Snakes & Ladders

A single self-contained `index.html` — no build step, no framework. Works with
Acode → GitHub → Vercel exactly like before.

## Features
- **Face-to-Face mode**: pass one phone around, 2–4 players, empty slots filled by bots.
- **Online mode**: create a room, get a 5-letter code, share it — friends join from
  their own phones and everyone sees the same board update live (powered by
  Firebase Firestore).
- **Normal difficulty**: friendly board, rolling a 6 always grants a bonus turn.
- **Hard difficulty**: nastier/more snakes, fewer ladders, you must roll the
  *exact* number to land on 100, and rolling a 6 only *sometimes* (~30% of the
  time) grants a bonus turn — instead of never, like before.
- Redrawn board: bigger numbers, a wood-style frame, a marked start/finish
  tile, and properly illustrated snakes (head, eyes, tongue, gradient body)
  and ladders (rails + rungs) instead of plain lines.

## 1. Set up Firebase (needed only for Online mode — Face-to-Face works with none of this)

Online play needs a tiny free database to store room codes and live game
state. Firebase's free tier is enough for this.

1. Go to https://console.firebase.google.com and create a new project
   (name it anything, e.g. `snakes-ladders`).
2. In the left sidebar, open **Build → Firestore Database → Create database**.
   Choose **Start in test mode** for now (easiest to get running — see the
   security note below for tightening it later).
3. In the left sidebar, click the gear icon → **Project settings**.
4. Under "Your apps", click the **</> (Web)** icon to register a new web app
   (any nickname is fine, no need to set up Hosting).
5. Firebase will show you a config object like this:
   ```js
   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "snakes-ladders-xxxx.firebaseapp.com",
     projectId: "snakes-ladders-xxxx",
     storageBucket: "snakes-ladders-xxxx.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcdef"
   };
   ```
6. Open `index.html`, find the `FIREBASE_CONFIG` object near the top of the
   `<script>` section, and paste your values in (replace all the
   `"YOUR_..."` placeholders).
7. Commit and push — Online mode will now work once deployed.

### Recommended Firestore security rules
Test mode allows anyone to read/write anything for 30 days, which is fine to
get started but too open long-term. Once things work, go to
**Firestore → Rules** and use something like:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /slgames/{code} {
      allow read, write: if true; // anyone with the code can play
    }
    match /players/{uid} {
      allow read, write: if true; // simple win/loss counters
    }
  }
}
```
This keeps things simple for a casual game (no login system) while still only
storing game rooms and basic stats — no personal data.

## 2. Deploy with Acode + GitHub + Vercel (same as before)
1. In Acode, keep `index.html` (and this `README.md`) in your project folder.
2. `git init && git add . && git commit -m "online snakes and ladders" && git push`
   to your GitHub repo.
3. Import the repo in Vercel. It's a static site — leave the build command
   empty and the output directory as `.` (root).
4. Open the deployed URL on any phone; share the room code with friends to
   play online, or use Face-to-Face mode on one device.

## Notes
- Room codes are 5 characters (e.g. `Q7K2M`) and expire only when you delete
  the document manually in Firestore — there's no automatic cleanup, so old
  test rooms will sit in your database (harmless, just clutter).
- The host's device drives the bots' turns in online mode, so keep the host's
  app open until the game finishes.
- No accounts/login — identity is just a random ID saved in the browser's
  local storage, so switching browsers/devices creates a "new" player.
