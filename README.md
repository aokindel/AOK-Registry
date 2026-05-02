# AOK Registry — Social Network UI

A Facebook-inspired social network web app with a three-column shell, news feed, stories, reactions, comments, notifications, messenger, search, and dark mode — all in a single self-contained HTML file.

## Features

- **Top navigation** — logo, search with type-ahead, tabbed nav, notifications, messenger, profile menu
- **Left rail** — profile shortcut, Friends / Groups / Marketplace / Watch / Memories / Saved / Pages / Events, your shortcuts
- **Right rail** — sponsored, birthdays, contacts with online indicators
- **Stories** — horizontal carousel + full-screen viewer with auto-advance, keyboard nav, pause
- **Post composer** — text + image upload (preview), audience picker, posts persist via localStorage
- **News feed** — text, single image, 2/3/4-image grids, link cards, sponsored
- **Reactions** — full 7-reaction picker (Like, Love, Care, Haha, Wow, Sad, Angry)
- **Comments** — expandable threads, write & post
- **Infinite scroll** — loads 5 more posts as you near the bottom
- **Dark mode** — toggle persists across sessions
- **Keyboard shortcuts** — `/` focuses search, `Esc` closes modals, `←/→` navigates stories
- **Zero dependencies** — single HTML file, no build step

## Run Locally

Just open `index.html` in any modern browser:

```bash
# macOS / Linux
open index.html

# Windows
start index.html

# Or serve with any static file server
npx serve .
python -m http.server 8080
```

## Deploy to Vercel

```bash
npm i -g vercel
cd "AOK Registry"
vercel
```

Or import the folder via the Vercel dashboard — `vercel.json` is included.

## Deploy to GitHub Pages

1. Push to GitHub
2. Settings → Pages → Source: `main` / root
3. Live at `https://<username>.github.io/<repo>/`

## Project Structure

```
AOK Registry/
├── index.html          # Complete app (HTML + CSS + JS)
├── vercel.json         # Vercel deployment config
├── .gitignore
├── README.md
├── SPEC.md             # Feature specification
└── IMPLEMENTATION.md   # Build plan
```

## Data Sources

- **Avatars**: `api.dicebear.com` — deterministic per seed, free, no key
- **Photos**: `picsum.photos` — placeholder images, deterministic by seed

If both are blocked / offline, avatars fall back to colored initials and image slots show a gradient placeholder. The app remains fully interactive offline.

## Customization

Open `index.html`, scroll to the `STATIC_DATA` block near the top of the `<script>`. Edit the `USERS`, `POSTS`, `STORIES`, `NOTIFICATIONS`, `MESSAGES`, `BIRTHDAYS`, `GROUPS`, or `MARKETPLACE` arrays to change the seed content.

To reset persisted state (created posts, reactions, comments, theme): open DevTools → Application → Local Storage → clear, or run `localStorage.clear()` in the console.

## Browser Support

Chrome 90+, Firefox 88+, Safari 14+, Edge 90+

---

Built with HTML, CSS, JavaScript · DiceBear · Picsum
