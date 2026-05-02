# AOK Registry — Social Network Web App

## Overview

A web-based social network UI inspired by Facebook's modern feed layout:
three-column shell (rails + feed), top navigation with search, stories
strip, post composer, infinite-scroll news feed, reactions, comments,
notifications, messenger, and dark mode.

---

## Feature Requirements

### Top Navigation

| Region | Contents |
|---|---|
| Left | Facebook logo, search box (with type-ahead dropdown) |
| Center | Tabbed nav: Home · Friends · Watch · Marketplace · Groups · Gaming |
| Right | Menu, Marketplace, Notifications, Messenger, Profile (with dropdown) |

### Left Rail (sticky)

- Profile shortcut (avatar + name)
- Friends, Groups, Marketplace, Watch, Memories, Saved, Pages, Events
- Your Shortcuts (groups, pages the user follows)
- Footer links (Privacy · Terms · Cookies · Ad Choices)

### Center Feed

1. **Stories carousel** — "Create Story" tile + 8+ friend story tiles
2. **Post composer** — input row + Live · Photo · Feeling buttons
3. **News feed** — chronological mix of:
   - Text-only posts
   - Photo posts (single image)
   - Multi-image posts (2x2 grid, "+N" overlay)
   - Link previews
   - Sponsored / Suggested for you
   - Friend-request suggestions inline
4. **Infinite scroll** — loads 5 more posts as user nears bottom

### Right Rail (sticky)

- Sponsored ad block (rotating)
- Birthdays (today + this week)
- Contacts list (online dot indicator)
- Group chats

### Post Card Anatomy

```
┌───────────────────────────────────────────────────┐
│ avatar  Author Name                       ··· ✕  │
│         2h · 🌍                                   │
├───────────────────────────────────────────────────┤
│ Text content...                                  │
│                                                  │
│ [ optional image / image grid ]                  │
├───────────────────────────────────────────────────┤
│ 👍❤️😆  Alice and 24 others       7 comments  3 shares │
├───────────────────────────────────────────────────┤
│   👍 Like      💬 Comment      ↗ Share           │
├───────────────────────────────────────────────────┤
│  avatar  Comment thread...                        │
│  [ Write a comment...                          🙂] │
└───────────────────────────────────────────────────┘
```

### Reactions

Long-press / hover on Like to reveal the reaction picker:
👍 Like · ❤️ Love · 🤗 Care · 😆 Haha · 😮 Wow · 😢 Sad · 😡 Angry

### Stories Viewer

Click a story to open full-screen viewer:
- Progress segments at top (one per story in the user's set)
- Auto-advance every 5 seconds
- Click left/right halves to navigate, ESC to close
- Tap-and-hold to pause (touch); spacebar pauses (desktop)

### Notifications & Messenger Dropdowns

- Click bell icon: panel of 6 recent notifications (likes, comments, friend requests)
- Click messenger icon: panel of recent chats with online status, unread badge

### Search Dropdown

Type-ahead matches across users, groups, and pages from seed data.

### Dark Mode

Toggle in profile dropdown; persisted in localStorage.

---

## Visual Style

| Token | Light | Dark |
|---|---|---|
| Background | `#f0f2f5` | `#18191a` |
| Card | `#ffffff` | `#242526` |
| Hover | `#f2f2f2` | `#3a3b3c` |
| Border | `#ced0d4` | `#3e4042` |
| Text primary | `#050505` | `#e4e6eb` |
| Text secondary | `#65676b` | `#b0b3b8` |
| Brand blue | `#1877f2` | `#2374e1` |
| Accent green | `#42b72a` | `#42b72a` |
| Accent red | `#f02849` | `#f02849` |

Typography: `'Segoe UI', 'Helvetica Neue', system-ui, sans-serif`
Border radius: `8px` (cards), `18px` (pill buttons), `50%` (avatars)
Shadow: `0 1px 2px rgba(0,0,0,0.1)` (light) / `none` (dark)

---

## Technical Architecture

### Tech Stack

| Layer | Choice | Reason |
|---|---|---|
| Runtime | Vanilla HTML/CSS/JS (single file) | Zero dependencies, runs in any browser |
| Avatars | DiceBear `avataaars` API (CDN URLs) | Free, no key, deterministic per seed |
| Photos | `picsum.photos` | Free placeholder images, deterministic by seed |
| Icons | Inline SVG | Pixel-perfect, themable via `currentColor` |
| Persistence | `localStorage` | Likes / posts / dark-mode survive refresh |

### Data Flow

```
Browser load
  └── seed `state` from STATIC_DATA + localStorage
        └── render(): paint feed / rails / stories from state
              └── user action (like/comment/post)
                    └── mutate state
                          └── re-render affected card
                                └── persist to localStorage
```

### Files

```
AOK Registry/
├── SPEC.md              ← this document
├── IMPLEMENTATION.md    ← phased build plan
├── README.md            ← quick-start
├── index.html           ← entire app (HTML + CSS + JS)
├── vercel.json          ← deploy config
└── .gitignore
```

---

## Non-Functional Requirements

- Works offline (no network calls except DiceBear/Picsum CDN images)
- No build step — open `index.html` directly
- Responsive at ≥1024px (single column collapses below)
- Accessible: keyboard nav for stories/dropdowns, ARIA labels on icon buttons
- Persists user-created posts & likes in localStorage
- Page weight target: <60 KB HTML (excluding lazy-loaded images)
