# Four Stops North

Family trip site — Toronto · Niagara · Angus · Montréal · Québec,
24 December 2026 – 9 January 2027. Nine people, one rental car.

**Live:** https://julianrobert04.github.io/canada-trip/

## Files

- `index.html` — the whole site. One self-contained file, no build step.
- `img/` — photos, one set of three per day, from Wikimedia Commons.
- `og.png` — 1200×630 link-preview card.

## How it is put together

The itinerary lives in the `DAYS` array near the top of the `<script>`
block at the bottom of `index.html`. Each day is an object:

| key | what it is |
|---|---|
| `opts` | the activity options, each with chips and a map pin (`ll`) |
| `eat`  | five restaurants |
| `shots`| three photos with caption, author and licence |
| `book` | optional booking panel — table, note, links |
| `stay` | optional hotel panel (day 16 only) |

Edit, save, commit, push. GitHub Pages redeploys in under a minute.

## Shared voting

Picks sync live between everyone looking at the page, through a Firebase
Realtime Database. There is **no Firebase SDK** — the database's REST
endpoint streams over `EventSource`, so the page loads no extra library.

- Database: `https://canada-trip-10b84-default-rtdb.firebaseio.com`
- Data shape: `/picks/{personId} = {name, items, updated}`
- `items` is a comma-separated list of `"{day}-{optionIndex}"`

The Firebase web config in `index.html` is public by design; security
comes from the database rules, which are scoped to `/picks` only and
reproduced in `firebase-rules.json` in this repo. If the rules are ever
lost, paste that file back into the Realtime Database → Rules tab.

The page works without the database: if it is unreachable the status dot
turns amber and picks save locally instead.

## Photos

Sourced from Wikimedia Commons under their respective free licences,
downsized to 760 px wide. Author and licence are credited under each
day's strip. To swap one, replace the file in `img/` and update the
matching entry in that day's `shots` array.

## Deploying

Nothing to build. Commit and push to `main`; GitHub Pages serves it.

The page is `noindex, nofollow` — unlisted, not private. Anyone with the
link can open it; search engines will not list it.
