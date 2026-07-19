# Americano

Site: https://americano.luiswo.dev

Round-robin tournament scheduler for **Americano**-format Padel, Tennis or Pickleball.

No dependencies, works offline, exclusively on your device. No account needed, no data collected. Open source and free to use.

## What is Americano?

Every team (or player) plays every other team exactly once. Points scored in each game accumulate across the tournament — final ranking is by total points, not wins. Games are played simultaneously across all available courts.

## Features

- **Singles or Teams mode** — toggle changes all labels throughout the app
- **Configurable courts** — matches within each round are assigned court numbers cycling 1 → N
- **Configurable game length** — set how many points per game (default 11)
- **Round-robin schedule** — generated via the circle method; every participant plays once per round
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
https://americano.luiswo.dev/?mode=mixer&players=Ann,Bob,Cleo,Dan&teamSize=2&rounds=6&courts=2
```

Parameters may sit in the query string (`?mode=mixer`) or after a `?` inside
the hash (`#rounds?mode=mixer`), whichever your host prefers.

| Param | Values | Effect |
|---|---|---|
| `mode` | `teams` \| `singles` \| `mixer` | Tournament mode |
| `players` | comma-separated names | Participant list (replaces existing) |
| `teams` | comma-separated names | Alias of `players` — same effect |
| `courts` | integer ≥ 1 | Number of courts |
| `teamSize` | integer ≥ 2 | Players per team (`mixer` only) |
| `rounds` | integer ≥ 1 | Rounds generated upfront (`mixer` only) |

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
- Legacy `#mixer`, `#singles`, `#teams` hash links still work.
