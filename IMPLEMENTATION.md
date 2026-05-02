# Implementation Plan — AOK Registry

## Phase 1 — Skeleton & Styles
**Goal**: Static three-column shell that looks like Facebook with no data yet.

1. **`index.html` semantic structure**:
   - `<header>` — logo, search, center nav tabs, right action icons, profile avatar
   - `<main>` grid: `360px 1fr 360px`
     - `<aside class="left-rail">` — profile shortcut, nav list, shortcuts, footer
     - `<section class="feed">` — stories strip, composer, post cards
     - `<aside class="right-rail">` — sponsored, birthdays, contacts
   - `<div id="story-viewer">` — full-screen modal (hidden by default)
   - `<div id="dropdowns">` — notifications, messenger, profile, search panels

2. **CSS variables** — light + dark palettes (see SPEC), toggled by `body[data-theme="dark"]`

3. **Component styles**:
   - `.card` — white bg, 8px radius, subtle shadow
   - `.icon-btn` — 40px round, hover bg
   - `.tab` — 110px wide, 4px bottom border on active
   - `.avatar` — 40px round; `.avatar-lg` 60px, `.avatar-sm` 32px
   - `.btn` (primary blue) / `.btn-ghost` / `.btn-pill`
   - `.story-tile` — 110×190, gradient overlay, name at bottom
   - Reaction picker — absolute positioned bubble with 7 emoji
   - Online dot — 10px green circle on bottom-right of avatar

4. **Sticky behavior** — left rail and right rail `position: sticky; top: 60px`; header `position: sticky; top: 0`

---

## Phase 2 — Data Layer
**Goal**: Realistic mock data baked into the page.

5. **`USERS` array** — 24 users with `{id, name, avatarSeed, online, gender}`

6. **`POSTS` array** — 18 seed posts mixing:
   - Text only
   - Single image (Picsum URL)
   - 2/3/4-image grids
   - Link card with title/description/thumb
   - Sponsored ads (different visual)
   Each post has `{id, authorId, time, text, images[], reactions{type:count}, userReaction, comments[]}`

7. **`STORIES` array** — 9 entries, each with 1–3 segment images

8. **`NOTIFICATIONS`, `MESSAGES`, `BIRTHDAYS`, `GROUPS`, `MARKETPLACE`** — seed each with 5–8 entries

9. **`state` object** — wraps all data, hydrates from `localStorage`, exposes `save()`

---

## Phase 3 — Feed Rendering & Interactivity
**Goal**: Functional feed with likes, comments, and posting.

10. **`renderFeed()`** — clears feed container, maps `state.posts` → card HTML

11. **`renderPost(post)`** — returns HTML string for a single post with proper image grid layout

12. **Reactions**:
    - Click `.like-btn` → toggle 👍 reaction, animate count
    - Hover/long-press `.like-btn` → show `.reaction-picker`
    - Click an emoji → set `post.userReaction`, update summary row

13. **Comments**:
    - Click `.comment-btn` → expand `.comments` section
    - Render thread; replies indented
    - `.comment-input` enter key appends a new comment authored by the user

14. **Composer**:
    - Click input → open modal: textarea, image upload (FileReader → dataURL), audience picker, Post button
    - Post button creates a new entry at top of `state.posts`, persists, re-renders

15. **Infinite scroll**:
    - `IntersectionObserver` on a `<div id="sentinel">`
    - Loads next 5 posts (cycles seed data with offset)

---

## Phase 4 — Stories
**Goal**: Carousel + full-screen viewer.

16. **Stories strip** — render 9 tiles with gradient overlay + author avatar in top-left + name at bottom

17. **Story viewer modal**:
    - Opens on click
    - Top: progress bar segments, one per image, auto-fill 5s each
    - Body: full-bleed image
    - Tap left half → previous segment, right half → next; ESC closes
    - Spacebar toggles pause

18. **"Create Story" tile** — first tile, blue + plus icon, opens a placeholder modal

---

## Phase 5 — Dropdowns & Search
**Goal**: Header interactivity.

19. **Notifications panel** — bell click toggles a panel anchored under the icon; lists 6 latest items with avatar + text + time

20. **Messenger panel** — chat icon click; lists 8 recent messages with online dot

21. **Profile dropdown** — avatar click; menu: View profile, Settings, Display & accessibility (with dark mode toggle), Help, Log out

22. **Search dropdown**:
    - On focus, show recent searches
    - On type, fuzzy-match across `USERS`, `GROUPS`, post text → grouped results

23. **Outside-click dismissal** — single document listener closes any open panel

---

## Phase 6 — Polish
**Goal**: Production-feel details.

24. **Dark mode** — toggle in profile menu, persisted, smooth transition

25. **localStorage persistence**:
    - User-created posts
    - Reaction state
    - Comments
    - Theme preference

26. **Keyboard shortcuts**:
    - `/` focuses search
    - `Esc` closes any modal/dropdown
    - `←/→` navigate stories

27. **Responsive** — at <1100px hide right rail; at <900px hide left rail; at <700px collapse header tabs

28. **Animations**:
    - Reaction picker fade-up
    - Card hover lift
    - Story progress bar smooth fill
    - Loading skeletons for "load more"

29. **Vercel config** — `vercel.json` with `cleanUrls: true`

30. **README** — run/deploy instructions

---

## Build Order Summary

```
Phase 1  →  Phase 2  →  Phase 3  →  Phase 4  →  Phase 5  →  Phase 6
Skeleton    Seed         Feed +       Stories     Dropdowns    Polish
+ CSS       data         likes/                    + search    + dark mode
            objects      comments
```

Estimated lines of code: ~2,000–2,500 (single file).
