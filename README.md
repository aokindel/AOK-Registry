# AOK Catalog

A politics-first social network. Facebook-style three-column layout, green palette, with **live political headlines pulled from the GDELT Doc API** mixed into the feed. Single self-contained HTML file.

## Features

- **Live political news in the feed** — top US headlines fetched on load from the [GDELT Doc API](https://api.gdeltproject.org/api/v2/doc/doc) (free, no key, CORS-enabled). News cards include source, image, headline, and a "Read the full story →" link to the original article. Inferred topic tag (Elections / Economy / Climate / etc.) per headline. Refresh button at the top of the feed.
- **Facebook-style layout** — sticky three-column shell with stories strip, composer, news feed, left/right rails
- **Green palette** — brand `#1d8a3e`, brand-soft `#e6f3ea`, soft greens throughout
- **Sign-up flow** — name + optional email + city/state
- **Compose your own takes** — text + photo upload + optional topic tag
- **Reactions** — full 7-emoji picker (Like, Love, Haha, Wow, Sad, Angry)
- **Comments** — open under any post (yours or a news headline)
- **Live polls** — one-vote-per-device with animated percentage bars
- **Election countdown** — days until Nov 3, 2026 midterms
- **Trending topics** — both as story tiles and right-rail rows
- **Sponsored slot** in right rail (revenue surface #1)
- **Catalog Pro tease** — $9/mo modal that hides ads when "subscribed" (revenue surface #2)
- **Search** — type-ahead across topics, headlines, and members
- **Dark mode** — toggle persists
- **Zero dependencies** — single HTML file, no build, no CDN, no API key (GDELT is free)

## How the news feed works

On every load (and when you click Refresh), the app calls:

```
https://api.gdeltproject.org/api/v2/doc/doc
  ?query=politics OR election OR congress OR senate OR "white house"
  &mode=artlist&maxrecords=20&format=json
  &sort=datedesc&sourcecountry=US&sourcelang=eng
```

It maps each article to a card with `{ source, title, image, url, time, issue }`. The issue tag is inferred from a regex over the headline text. If the API request fails (offline, blocked, or rate-limited), the feed shows your own posts only — no fake fallback content.

GDELT updates roughly every 15 minutes and indexes thousands of news outlets globally. To narrow or change the focus, edit the `query` string in `fetchPoliticalNews()`.

## Swap or extend the news source

The `fetchPoliticalNews()` function is the only integration point. To use a different API (NewsAPI, Guardian, Mediastack, an RSS proxy via allorigins.win, etc.), replace its body — keep the same return shape (`state.newsArticles` filled with `{ id, kind: "news", source, title, image, url, time, issue }` objects).

## Monetization model (modeled, not active)

The UI demonstrates two revenue paths a politics-themed publisher commonly uses:

1. **Display ads** — the right-rail "Sponsored" card is a placeholder for an ad-network slot (Google AdSense, Mediavine, etc.). To turn it on, swap the static placeholder for the network's script tag and apply for inclusion. Political content can have lower CPMs and stricter policies — read each network's content rules before integrating.
2. **Premium subscription** — the "Catalog Pro · $9/mo" banner and modal model a subscription tier (ad-free + exclusive content). To turn it on, integrate Stripe Checkout (or Paddle/Lemon Squeezy) and gate the sponsored block + a premium-content collection behind the `state.isPro` flag (already in place in `index.html`).

**Honest note on the strategy.** Politics-first engagement businesses face well-documented risks around polarization, rage-farming, and misinformation amplification. The serious players in this category — *RealClearPolitics*, *The Dispatch*, *Lawfare*, *Vox*, *FiveThirtyEight* — invest heavily in editorial standards, sourcing, and balance, because that's what makes audiences durable rather than just viral. If you want this to last, the moat is editorial quality, not outrage volume. The chrome here (topic tags, balanced palette, factual countdown) leans toward that direction; the rest is up to what you publish.

## Important: still frontend-only

User accounts, posts, comments, and reactions live in the browser's `localStorage`. Friends on different devices won't see each other's posts until you add a backend (Supabase, Firebase, Vercel KV/Postgres). The frontend is structured so the persistence layer can be swapped without rewriting the UI. **The news feed itself is global** — every visitor sees the same headlines because they come from a public API.

## Run Locally

```powershell
# Windows
start index.html

# Or serve over http
python -m http.server 8080
```

```bash
# macOS / Linux
open index.html
# or
npx serve .
```

## Deploy

### Vercel
```bash
npm i -g vercel
cd "AOK Registry"
vercel
```
Or via the dashboard: https://vercel.com/new → Import `aokindel/AOK-Registry` → Deploy.

### GitHub Pages
Settings → Pages → Source `main` / `/` → live at `https://aokindel.github.io/AOK-Registry/`

## Project Structure

```
AOK Registry/
├── index.html          # Complete app (HTML + CSS + JS)
├── vercel.json
├── .gitignore
├── README.md
├── SPEC.md
└── IMPLEMENTATION.md
```

## Reset

`localStorage.clear()` in the console wipes accounts, posts, and Pro state.

## Browser Support

Chrome 90+, Firefox 88+, Safari 14+, Edge 90+
