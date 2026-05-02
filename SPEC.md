# AOK Registry — Specification

## Overview

A green-and-white social network UI. Visitors sign up on first visit, then post, react, and comment. All state is stored in `localStorage` — no backend.

---

## Feature Requirements

### Welcome / Sign-up

- First visit: full-screen overlay with branded header, name (required), email + bio (optional)
- "Already have an account on this device?" → sign-in mode that picks an existing local account by name
- All accounts on the device are stored together; current logged-in user is `state.me`
- Sign out clears `state.me` and re-shows the welcome overlay

### Top Navigation

| Region | Contents |
|---|---|
| Left | AOK logo, "Registry" wordmark, search box (type-ahead) |
| Center | Tabs: Home · People · Groups · Saved |
| Right | Notifications, Profile (with dropdown) |

### Left Rail

- Profile shortcut (avatar + name)
- People, Groups, Memories, Saved, Events
- Invite-friends prompt with the share-this-URL hint
- Footer: Privacy / Terms / About

### Center Feed

1. **Stories carousel** — Create-story tile + placeholder slots (no fake stories)
2. **Post composer** — input, Photo / Tag / Feeling buttons
3. **News feed** — user-created posts only, with rich empty state when empty

### Right Rail

- People list — every other account on this device
- Quick-link section (Create post, Find people, Notifications)

### Post Card

```
┌──────────────────────────────────────────┐
│ avatar  Author Name                ··· ✕ │
│         2h · 🌍                          │
├──────────────────────────────────────────┤
│ Text content                             │
│ [optional uploaded image]                │
├──────────────────────────────────────────┤
│ 👍❤️😆 24            5 comments          │
├──────────────────────────────────────────┤
│   👍 Like     💬 Comment     ↗ Share     │
├──────────────────────────────────────────┤
│  avatar  Comment thread...               │
│  [Write a comment...                    ]│
└──────────────────────────────────────────┘
```

### Reactions

Hover the Like button to reveal: 👍 Like · ❤️ Love · 🤗 Care · 😆 Haha · 😮 Wow · 😢 Sad · 😡 Angry. Counts and the user's choice persist in `localStorage`.

### Edit Profile

Profile dropdown → "Edit profile" → modal with name, email, bio. Saving updates both `state.me` and the entry in `state.users`.

### Notifications

A welcome notification is generated on first sign-up. Future expansion can append to `state.notifications` for likes/comments. Unread count appears as a badge on the bell icon.

### Search

- Empty query: shows local accounts
- Typed query: matches on user names + post text, grouped as "People" and "Posts"

### Dark Mode

Toggle in profile dropdown; persisted in `state.theme`.

---

## Visual Style

| Token | Light | Dark |
|---|---|---|
| Background | `#f3f7f4` | `#0e1612` |
| Card | `#ffffff` | `#1a221c` |
| Hover | `#ecf2ed` | `#2a342d` |
| Border | `#cdd8d1` | `#2c3830` |
| Text primary | `#0d1c12` | `#e2ebe5` |
| Text secondary | `#4d5e54` | `#a3b3a8` |
| Brand green | `#1d8a3e` | `#2db05c` |
| Accent red (love/notif) | `#d23a4d` | `#d23a4d` |

Typography: `'Segoe UI', 'Helvetica Neue', system-ui, sans-serif`
Avatars: locally-generated initials on a hue-hashed colored background — no third-party avatar service.

---

## Technical Architecture

### Tech Stack

| Layer | Choice | Reason |
|---|---|---|
| Runtime | Vanilla HTML/CSS/JS, single file | Zero deps, no build, no CDN |
| Storage | `localStorage` (`aok-registry-state-v1`) | Survives refresh; private to each browser |
| Avatars | Inline initials + HSL hash of name | No external service; fully offline |
| Icons | Inline SVG | Themable via `currentColor` |

### State Shape

```js
{
  me: { id, name, email, bio, color, joinedAt } | null,
  users: [ { id, name, email, bio, color, joinedAt } ],
  posts: [ { id, authorId, time, text, images } ],
  reactions: { [postId]: reactionId },
  reactionCounts: { [postId]: { [reactionId]: number } },
  comments: { [postId]: [ { id, authorId, text, time } ] },
  notifications: [ { id, text, time, unread } ],
  theme: "light" | "dark"
}
```

### Files

```
AOK Registry/
├── SPEC.md              ← this document
├── index.html           ← entire app (HTML + CSS + JS)
├── IMPLEMENTATION.md    ← phased build plan
└── README.md            ← run / deploy
```

---

## Non-Functional Requirements

- Fully offline (no network calls of any kind)
- No build step
- Responsive at ≥1024px (rails collapse below)
- Accessible: keyboard nav for dropdowns/modals, ARIA-friendly markup
- Honest: a banner in the welcome dialog and README notes that state is per-device only

---

## Future Work (when a backend is added)

- Replace `localStorage` reads/writes with API calls (Supabase Auth + Postgres or Firebase)
- Email verification on sign-up
- Friend requests + privacy controls
- Real stories with auto-expire
- Real cross-device notifications
