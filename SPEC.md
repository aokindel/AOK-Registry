# AOK Catalog — Specification

## Overview

A politics-themed community feed. Members sign up, follow topics, share takes tagged by issue, react and comment, and see trending topics, live polls, and an election countdown alongside their feed. Two monetization surfaces are wired in (sponsored slot, premium subscription tease) but inactive — they are placeholders for AdSense + Stripe integration.

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
| Left | AOK navy-circle logo, "Catalog" wordmark, search box (type-ahead) |
| Center | Tabs: News · Topics · Candidates · Polls · Op-Eds |
| Right | "GO PRO" CTA button, Notifications, Profile dropdown |

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
| Background | `#faf8f3` (newsprint cream) | `#14181f` |
| Card | `#ffffff` | `#1c2230` |
| Hover | `#efebe0` | `#2a3142` |
| Border | `#dad6cb` | `#2c3343` |
| Text primary | `#1a1a1a` | `#e6e9ef` |
| Brand navy | `#0f2f6e` | `#5286e0` |
| Accent red | `#c1272d` | `#e35861` |
| Civic green | `#2f8c4a` | `#2f8c4a` |
| Civic gold | `#b08a2a` | `#b08a2a` |

Typography:
- Headlines / brand: `Georgia, 'Times New Roman', serif`
- Body / UI: `'Segoe UI', 'Helvetica Neue', system-ui, sans-serif`

Avatars: locally-generated initials on hue-bucketed civic-palette backgrounds (navy/red/gold/teal).

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

- `ISSUES` — 12 hard-coded issue categories with color classes
- `TRENDING` — 5 curated topic rows (right rail)
- `POLLS` — single seeded poll with realistic baseline counts
- `ELECTION` — fixed date (Nov 3, 2026)
- No fake users, no fake posts, no fake comments

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
