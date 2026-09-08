# 🐍 Snakes & Ladders

A single self-contained `index.html` — no build step, no framework

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

## Notes
- Room codes are 5 characters (e.g. `Q7K2M`) and expire only when you delete
  the document manually in Firestore — there's no automatic cleanup, so old
  test rooms will sit in your database (harmless, just clutter).
- The host's device drives the bots' turns in online mode, so keep the host's
  app open until the game finishes.
- No accounts/login — identity is just a random ID saved in the browser's
  local storage, so switching browsers/devices creates a "new" player.
