# AOK Registry

A clean, green-and-white social network UI. Sign up, share posts, react and comment, all from a single self-contained HTML file.

## Features

- **Welcome / sign-up flow** — first visitor enters name (+ optional email/bio) and becomes the device's account
- **Multi-account on one device** — friends visiting on the same browser can each create their own account and switch between them
- **Post composer** — text + image upload, persists to localStorage
- **Reactions** — full 7-emoji picker (Like, Love, Care, Haha, Wow, Sad, Angry)
- **Comments** — expandable threads, write & post
- **News feed** with empty state when there are no posts yet
- **People rail** — every account on this device shows up here
- **Notifications** with unread badge
- **Search** — type-ahead across people on this device + post text
- **Edit profile** — change name, email, bio anytime
- **Dark mode** — toggle in profile menu, persists
- **Keyboard shortcuts** — `/` focuses search, `Esc` closes modals
- **Zero dependencies** — single `index.html`, no build step, no external CDNs

## Important: this is frontend-only

All accounts and posts are stored in your **browser's localStorage**. That means:

- Each visitor on each device has their own private feed.
- Friends on different devices **cannot see each other's posts** unless they are at the same browser.
- Clearing browser data wipes the account.

To make this a real social network where friends on different devices can see each other's posts, you'd need to wire up a backend (Supabase, Firebase, or a serverless KV store like Vercel KV). The frontend is structured so you could swap localStorage for an API client without rewriting the UI.

## Run Locally

```powershell
# Windows
start index.html

# Or serve over http (recommended)
python -m http.server 8080
# then visit http://localhost:8080
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
1. Push to GitHub
2. Repo Settings → Pages → Source: `main` / `/` (root)
3. Live at `https://<username>.github.io/AOK-Registry/`

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

## Reset

To wipe local accounts/posts and start fresh:
- DevTools → Application → Local Storage → clear, or
- Run `localStorage.clear()` in the console

## Browser Support

Chrome 90+, Firefox 88+, Safari 14+, Edge 90+
