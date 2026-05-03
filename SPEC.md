# AOK Catalog — Specification

## Overview

A politics-themed social network with Facebook-style layout, green palette, and **a live political news feed pulled from the GDELT Doc API**. Members sign up, react and comment on headlines, share their own takes (optionally tagged with an issue), and see trending topics, a live poll, and an election countdown alongside the feed.

---

## Feature Requirements

### Sign-up

- Welcome overlay (full-screen) with brand panel
- Required: name. Optional: email, city/state, picked issues
- Sign-in mode for returning visitors (matches by name on this device)
- All accounts on the device live in `state.users`; current user is `state.me`

### Top Navigation

| Region | Contents |
|---|---|
| Left | AOK green-circle logo, "Catalog" wordmark, search box (type-ahead) |
| Center | Tabs (icon-style, FB-like): Home · News · Topics · Polls |
| Right | "Go Pro" CTA button, Notifications, Profile dropdown |

### Left Rail

- Profile shortcut
- Browse: News feed · Trending · Issues · Candidates · Polls · Congress watch · Saved articles
- Your issues — chips of the topics this user follows
- Footer: Privacy / Terms / Editorial standards

### Center Feed

- Composer with three actions: Photo · Link · Topic
- Composer modal includes a topic-tag picker (chips for all 12 issues)
- Posts render with optional issue chip below the author row
- Empty state when no posts

### Right Rail

- **Election countdown** — large card to next U.S. midterm (Nov 3 2026)
- **Live poll** — current question with one-vote-per-device, animated percentage fills
- **Trending today** — curated list of topical sections
- **Sponsored card** — gold-bordered, clearly labeled ad placeholder (hidden if `state.isPro`)

### Reactions

`👍 Up · ❤️ Strong agree · 😆 Haha · 😮 Whoa · 😢 Sad · 😡 Strong disagree`

The labels are tuned for civic discourse — "Strong agree / Strong disagree" replaces "Love / Angry" to encourage positional rather than emotional signaling.

### Comments

Threaded under each post; uppercase action labels (Up · Reply · timestamp) for newsprint feel.

### Catalog Pro

Modeled subscription tier. "GO PRO" header CTA, banner inside the profile dropdown, and a dedicated modal with feature list ($9/mo). Subscribing (demo) flips `state.isPro = true` and hides the sponsored slot.

### Edit Profile

Profile dropdown → Edit profile → modal with name, email, city/state, issues picker.

---

## Visual Style

| Token | Light | Dark |
|---|---|---|
| Background | `#f0f4f1` | `#0e1612` |
| Card | `#ffffff` | `#1a221c` |
| Hover | `#ecf2ed` | `#2a342d` |
| Border | `#cdd8d1` | `#2c3830` |
| Text primary | `#0d1c12` | `#e2ebe5` |
| Brand green | `#1d8a3e` | `#2db05c` |
| Accent red (love/notif) | `#d23a4d` | `#d23a4d` |
| Gold (haha/wow/sad reactions) | `#b8860b` | `#b8860b` |

Typography: `'Segoe UI', 'Helvetica Neue', system-ui, sans-serif` (no serif). FB-style chrome.

Avatars: locally-generated initials on hue-bucketed colored backgrounds.

---

## Technical Architecture

### State Shape

```js
{
  me: { id, name, email, bio, issues[], color, joinedAt } | null,
  users: [...],
  posts: [ { id, authorId, time, text, images[], issue } ],
  reactions: { [postId]: reactionId },
  reactionCounts: { [postId]: { [reactionId]: number } },
  comments: { [postId]: [...] },
  notifications: [...],
  votes: { [pollId]: optionId },
  pollCounts: { [pollId]: { [optionId]: number } },
  isPro: boolean,
  theme: "light" | "dark"
}
```

Stored in `localStorage` under `aok-catalog-state-v1`.

### Data

- `ISSUES` — 10 hard-coded issue categories with color classes
- `TRENDING` — 5 curated topic rows (right rail + story tiles)
- `POLLS` — single seeded poll with realistic baseline counts
- `ELECTION` — fixed date (Nov 3, 2026)
- **Live news** — `fetchPoliticalNews()` calls GDELT Doc API on load + on Refresh button click. Articles are NOT persisted; they re-fetch each session.
- No fake users, no fake user posts, no fake comments

### News API Integration

```
GET https://api.gdeltproject.org/api/v2/doc/doc
  ?query=politics OR election OR congress OR senate OR "white house"
  &mode=artlist&maxrecords=20&format=json
  &sort=datedesc&sourcecountry=US&sourcelang=eng
```

Response shape `{ articles: [{ url, title, seendate, socialimage, domain, ... }] }`.
Each article is mapped to `{ id, kind: "news", source, title, image, url, time, issue }`.
Topic (`issue`) is inferred via regex over the headline text.

### Files

```
AOK Registry/
├── SPEC.md
├── IMPLEMENTATION.md
├── README.md
├── index.html
├── vercel.json
└── .gitignore
```

---

## Monetization Hooks

| Surface | State | What's needed to activate |
|---|---|---|
| Sponsored card | Static placeholder | Drop in AdSense script + apply for approval |
| Catalog Pro modal | Modeled UI; `state.isPro` flips ad visibility | Add Stripe Checkout link, gate `state.isPro` on real payment webhook |
| Premium content | None yet | Add a `premium: true` flag on posts and a paywall component |

---

## Non-Functional Requirements

- Fully offline (no network calls)
- No build step; single file
- Responsive at ≥1180px (rails collapse below)
- Honest disclosure: README and welcome dialog flag local-only state and unactivated ad surfaces
- Civic chrome: balanced palette, neutral category names, factual data (real election date)
