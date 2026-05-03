# AOK Catalog

> *Politics, decoded — for citizens, not algorithms.*

A politics-first community web app: news feed, topic tags, candidate browsing, live polls, election countdown, and member-driven discussion. Single self-contained HTML file. Navy / red / cream civic palette.

## Features

- **Sign-up flow** — name, email, location, plus an "issues you follow" picker
- **Topic-tagged posts** — every post can be tagged with an issue (Economy, Healthcare, Foreign Policy, Climate, Civil Rights, Elections, Education, Immigration, Tech & AI, Housing, Courts, Energy)
- **Civic-tuned reactions** — Up · Strong agree · Haha · Whoa · Sad · Strong disagree
- **Comments** — threaded discussion under every post
- **Live polls** — one-vote-per-device, real-time percentages with seeded baseline counts
- **Election countdown** — to the next U.S. midterm (Nov 3, 2026)
- **Trending topics** — curated topic categories surfaced in the right rail
- **Sponsored slot** — clearly labeled ad placement (revenue surface #1)
- **Catalog Pro tease** — premium subscription modal at $9/mo, ad-free + premium analysis (revenue surface #2)
- **Search** — type-ahead across topics, members, and posts
- **Dark mode** — toggle persists
- **Empty states** throughout — no fake people, no synthetic content
- **Zero dependencies** — single HTML file, no build, no CDN, no API keys

## Monetization model (modeled, not active)

The UI demonstrates two revenue paths a politics-themed publisher commonly uses:

1. **Display ads** — the right-rail "Sponsored" card is a placeholder for an ad-network slot (Google AdSense, Mediavine, etc.). To turn it on, swap the static placeholder for the network's script tag and apply for inclusion. Political content can have lower CPMs and stricter policies — read each network's content rules before integrating.
2. **Premium subscription** — the "Catalog Pro · $9/mo" banner and modal model a subscription tier (ad-free + exclusive content). To turn it on, integrate Stripe Checkout (or Paddle/Lemon Squeezy) and gate the sponsored block + a premium-content collection behind the `state.isPro` flag (already in place in `index.html`).

**Honest note on the strategy.** Politics-first engagement businesses face well-documented risks around polarization, rage-farming, and misinformation amplification. The serious players in this category — *RealClearPolitics*, *The Dispatch*, *Lawfare*, *Vox*, *FiveThirtyEight* — invest heavily in editorial standards, sourcing, and balance, because that's what makes audiences durable rather than just viral. If you want this to last, the moat is editorial quality, not outrage volume. The chrome here (topic tags, balanced palette, factual countdown) leans toward that direction; the rest is up to what you publish.

## Important: still frontend-only

All accounts and posts live in the browser's `localStorage`. Friends on different devices won't see each other's content until you add a backend (Supabase, Firebase, Vercel KV/Postgres). The frontend is structured so the persistence layer can be swapped without rewriting the UI.

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
