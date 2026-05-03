# Implementation Plan — AOK Catalog

## Phase 1 — Skeleton & Civic Identity
**Goal**: Three-column shell with newspaper / civic feel.

1. **Header** — navy AOK circle logo, "Catalog" wordmark in serif, search ("Search the Catalog"), 5 serif tabs (News · Topics · Candidates · Polls · Op-Eds), red "GO PRO" button, Notifications, Profile
2. **CSS variables** — newsprint-cream bg `#faf8f3`, navy brand `#0f2f6e`, accent red `#c1272d`, civic green/gold; serif headers via Georgia
3. **Welcome screen** — navy gradient backdrop with red accent border on the panel; brand name with red accent on "Catalog"

---

## Phase 2 — State & Persistence
**Goal**: Honest data layer with two new dimensions: `votes` and `isPro`.

4. **State shape** adds `votes`, `pollCounts`, `isPro` on top of the previous account/post structure
5. **`ISSUES`** — 12 hard-coded categories with color classes (`accent`, `green`, `gold`, base navy)
6. **`TRENDING`** — 5 seeded topic rows
7. **`POLLS`** — single live poll with baseline option counts
8. **`ELECTION`** — `2026-11-03T08:00:00-05:00`

---

## Phase 3 — Sign-up with Issues
**Goal**: Civic-flavored onboarding.

9. **Welcome panel** — name, email, city/state, "issues you follow" multi-select chip grid
10. **Issues picker** — reusable function used in welcome and edit-profile
11. **Welcome notification** — first-time signup adds a one-line greeting to `state.notifications`

---

## Phase 4 — Feed with Topic Tags
**Goal**: Posts carry an issue tag.

12. **Composer modal** — adds `pendingTopic` and a topic chip picker; sends `issue` field on submit
13. **`postHTML`** — renders the issue chip in a `topic-row` between the head and the body
14. **Civic reactions** — picker emoji set unchanged; labels swapped to Up / Strong agree / Haha / Whoa / Sad / Strong disagree

---

## Phase 5 — Right-rail Civic Surfaces
**Goal**: Election countdown, live poll, trending, sponsored.

15. **Election countdown** — derive days from `Date.now()` to `ELECTION.date`
16. **Live poll** — render options with animated `.fill` width based on percentages; one vote per device via `state.votes[pollId]`
17. **Trending list** — five-row card with rank + section label + topic + stat
18. **Sponsored card** — gold-bordered placeholder, hidden when `state.isPro`

---

## Phase 6 — Monetization Surfaces
**Goal**: Two activatable revenue paths.

19. **GO PRO button** in header → opens Pro modal
20. **Pro banner** in profile dropdown → opens Pro modal
21. **Pro modal** — feature list, $9/mo headline, "Subscribe (demo)" button → `state.isPro = true`, re-renders right rail (hides sponsored slot)
22. **Edit profile modal** with name / email / city-state / issues picker

---

## Phase 7 — Polish
**Goal**: Production feel.

23. **Search** — type-ahead grouped: Topics · People · Posts
24. **Dark mode** — toggle persists; preserves civic identity
25. **Lightbox** — click any image to view full
26. **Keyboard** — `/` focuses search, `Esc` closes any modal/dropdown
27. **Responsive** — collapse rails at 1180 / 980 / 800px
28. **Empty states** — newsprint-feel "Your feed is quiet" card

---

## Build Order Summary

```
Phase 1   →  Phase 2   →  Phase 3   →  Phase 4   →  Phase 5   →  Phase 6   →  Phase 7
Skeleton     State /     Sign-up      Topic-tag    Civic        Pro /       Search /
+ civic      polls /     + issues     posts        rail         sponsored   dark /
identity     election                              widgets      surfaces    polish
```

Single-file, ~1,800 lines, no external services.
