# Americano

Site: https://americano.luiswo.dev

Round-robin tournament scheduler for **Americano**-format Padel, Tennis or Pickleball.

No dependencies, works offline, exclusively on your device. No account needed, no data collected. Open source and free to use.

## What is Americano?

Every team (or player) plays every other team exactly once. Points scored in each game accumulate across the tournament — final ranking is by total points, not wins. Games are played simultaneously across all available courts.

## Features

- **Singles or Teams mode** — toggle changes all labels throughout the app
- **Courts assigned on start** — no court count to configure; a match gets the lowest free court number when you tap *Start* (or *New game*)
- **Court-by-court scheduling** — no rounds: tap *New game* whenever a court frees up. Players/teams waiting longest (and with fewest games) go next, avoiding repeat match-ups. Works in every mode
- **Configurable game length** — set how many points per game (default 11)
- **Round-robin schedule** — one round at a time via *Add round*; follows the circle method, so every participant plays every other exactly once per cycle before a new shuffled cycle starts
- **Bye handling** — odd number of participants? one sits out each round, clearly shown
- **Score entry** — tap a round, enter scores, save; edit anytime
- **Live standings** — sorted by W=3, D=1, L=0 points; tie-break by `points_scored - points_conceded`
- **Offline-first** — no network needed after first load
- **Persistent** — scores survive page close via `localStorage`, all on-device
- **URL configuration** — pre-fill a whole tournament from a link (see below)

## Configure via URL

Any setting can be pre-filled with URL parameters, so an external project can
link straight into a prepared setup — the user only presses *Generate
schedule*. There is no UI for this — you build the link yourself.

```
https://americano.luiswo.dev/?mode=mixer&players=Ann,Bob,Cleo,Dan&teamSize=2
```

Parameters may sit in the query string (`?mode=mixer`) or after a `?` inside
the hash (`#rounds?mode=mixer`), whichever your host prefers.

| Param | Values | Effect |
|---|---|---|
| `mode` | `teams` \| `singles` \| `mixer` | Tournament mode |
| `players` | comma-separated names | Participant list (replaces existing) |
| `teams` | comma-separated names | Alias of `players` — same effect |
| `teamSize` | integer ≥ 2 | Players per team (`mixer` only) |
| `rolling` | `1` \| `0` (also `true`/`false`) | Court-by-court scheduling on/off (off = fixed rounds) |

Notes:

- `players` and `teams` are interchangeable in every mode; both fill the same
  list and may even be combined. Use whichever reads better for your `mode`.
- Names are URL-encoded like any parameter — `players=Ann%20Lee,Bob`. Repeating
  the parameter also works: `players=Ann&players=Bob`.
- Invalid or out-of-range values are ignored, keeping the current setting.
- Passing `players`/`teams` clears any existing schedule and scores, since the
  participant list changed. The other params leave an existing schedule alone.
- **A config is applied once per distinct parameter set.** The set is
  fingerprinted into `localStorage`, so reloading or re-opening the same link
  does not wipe scores already entered. Change any parameter to apply it again.
- `courts` and `rounds` are no longer supported and are ignored — courts are assigned when a match starts, and mixer generates one round with more added on the fly.
- Legacy `#mixer`, `#singles`, `#teams` hash links still work.
