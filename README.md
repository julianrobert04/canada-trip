# Four Stops North

Family trip site — Toronto · Angus · Montréal · Québec, 24 Dec 2026 – 9 Jan 2027.

## Files

- `index.html` — the whole site. One self-contained file, no build step, no dependencies.
- `og.png` — 1200×630 link-preview card (WhatsApp, iMessage, Facebook, Twitter).
- `README.md` — this.

## Before going live

Open `index.html` and replace every `https://REPLACE-WITH-YOUR-DOMAIN`
with the real address. It appears 5 times, all inside `<head>`.

```bash
cd ~/canada-trip
sed -i '' 's|https://REPLACE-WITH-YOUR-DOMAIN|https://julianrobert04.github.io/canada-trip|g' index.html
```

Only the link-preview card and the canonical URL depend on this. The site
itself works without it.

## To keep it out of Google

Add this inside `<head>`:

```html
<meta name="robots" content="noindex, nofollow">
```

The page is still reachable by anyone with the link — this only stops it
being indexed and found by search.

## Deploying

Any static host works. The whole site is two files.

### GitHub Pages
```bash
cd ~/canada-trip
git init && git add -A && git commit -m "Four Stops North"
gh repo create canada-trip --public --source=. --push
gh api -X POST repos/:owner/canada-trip/pages -f "source[branch]=main" -f "source[path]=/"
```
Live at `https://<user>.github.io/canada-trip/` in about a minute.
Note: Pages from a **private** repo needs a paid GitHub plan.

### Cloudflare Pages
Connect the repo at dash.cloudflare.com → Workers & Pages → Create → Pages.
Build command: leave empty. Output directory: `/`.

### Netlify
Drag this folder onto https://app.netlify.com/drop. No account needed to start.

## Custom domain

Buy the domain (Cloudflare Registrar is at-cost, ~$10–12/yr), then:

- **GitHub Pages** — add the domain under repo Settings → Pages, create a
  `CNAME` file containing the domain, and point a DNS `CNAME` record at
  `<user>.github.io`.
- **Cloudflare Pages / Netlify** — add the domain in their dashboard and
  follow the DNS instructions they generate.

HTTPS is automatic and free on all three.

## Editing later

It is one HTML file. The itinerary lives in the `DAYS` array near the top of
the `<script>` block at the bottom — each day is an object with `opts`
(the activity options) and `eat` (the restaurants). Edit, save, redeploy.
