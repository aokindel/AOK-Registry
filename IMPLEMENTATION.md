# Implementation Plan — AOK Registry

## Phase 1 — Skeleton & Styles
**Goal**: Static three-column shell with the green/white identity.

1. **`index.html` semantic structure**:
   - `<header>` — AOK logo (green circle, white "AOK"), "Registry" wordmark, search, 4 tabs (Home · People · Groups · Saved), notifications + profile
   - `<main>` grid: `360px 1fr 360px`
     - `<aside class="left-rail">`
     - `<section class="feed">`
     - `<aside class="right-rail">`
   - Welcome overlay (full-screen on first visit)
   - Composer modal, Edit-profile modal, Story viewer, Lightbox

2. **CSS variables** — green palette in `:root`, dark equivalents in `body[data-theme="dark"]`

3. **Component styles** — card, avatar (initials only, no img), buttons, dropdowns, modals, story tiles, reaction picker, comments, welcome panel

---

## Phase 2 — State & Persistence
**Goal**: Honest data layer with no fake content.

4. **State shape** — `me`, `users`, `posts`, `reactions`, `reactionCounts`, `comments`, `notifications`, `theme`. No seed data. Empty arrays/objects on first launch.

5. **Persistence** — `hydrate()` reads `localStorage[aok-registry-state-v1]` into `state`; `persist()` writes the whole object. Called after every mutation.

6. **Initials avatar generator** — `colorForName()` hashes each char into an HSL color; `initialsOf()` returns up to 2 uppercase initials.

---

## Phase 3 — Welcome / Sign-up
**Goal**: First-visit account creation + sign-in.

7. **Welcome overlay** — green gradient backdrop, branded panel, name + email + bio fields
8. **Mode switcher** — toggle between "Create account" and "Sign in" (re-uses existing local account by name match)
9. **`createAccount()`** — validates name, generates user object, pushes onto `state.users`, sets `state.me`, persists, hides overlay, calls `renderAll()`
10. **`signOut()`** — clears `state.me`, persists, re-shows overlay
11. **Welcome notification** — first-time signup adds an entry to `state.notifications`

---

## Phase 4 — Feed & Interactivity
**Goal**: Posts, reactions, comments.

12. **`renderFeed()`** — empty-state card when no posts; otherwise maps `state.posts` → `postHTML()`
13. **`postHTML(post)`** — header (avatar + author + relTime + delete-if-mine), body (text + images), summary row (top reaction emojis + count + comment count), action row (Like/Comment/Share), comments thread
14. **Reactions**:
    - Hover Like for 350ms → reaction picker fades up
    - Click an emoji → `setReaction()` mutates `state.reactionCounts[postId]` and `state.reactions[postId]`, decrements previous if any
    - Click Like body → toggle current reaction (or default to like)
15. **Comments**:
    - Click Comment button → expand `.comments` section, focus input
    - Enter or send button → push comment to `state.comments[postId]`, re-render post
16. **Composer**:
    - Click input → modal with refreshed avatar, textarea, image upload (FileReader → dataURL preview)
    - Submit → unshift onto `state.posts`, persist, re-render feed
17. **Delete post** — visible only on author's own posts

---

## Phase 5 — Stories, Search, Profile
**Goal**: Surrounding surfaces.

18. **Stories carousel** — Create-story tile + 3 placeholder slots (no fake stories until a real story flow is implemented). Click Create-story → opens composer.
19. **Search** — type-ahead. Empty query lists local accounts; typed query matches user names + post text, grouped.
20. **Edit profile** — modal pre-fills name/email/bio, save updates both `state.me` and the entry in `state.users`, recolors avatar.
21. **Notifications dropdown** — list with unread states; clicking the bell marks all read.

---

## Phase 6 — Polish
**Goal**: Production feel.

22. **Dark mode** — toggle in profile menu, persisted, smooth transitions
23. **Lightbox** — click any image in feed to view full-size
24. **Keyboard shortcuts** — `/` focuses search, `Esc` closes modals
25. **Responsive breakpoints** — 1300 / 1100 / 900 / 700px
26. **Outside-click dismiss** for all dropdowns
27. **Animations** — reaction-picker fade-up, like-button pop on react, story-tile hover scale

---

## Build Order Summary

```
Phase 1   →  Phase 2   →  Phase 3   →  Phase 4   →  Phase 5   →  Phase 6
Skeleton     State /     Welcome /     Feed /       Stories /    Polish
+ CSS        persist     sign-up       reactions    search /     + dark
                                       comments     profile      mode
```

Single file, ~1,500 lines, no external services.
