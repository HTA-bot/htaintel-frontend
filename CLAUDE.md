# HTA Intel Frontend — CLAUDE.md

See the backend repo CLAUDE.md for full project context, architecture, and history.
Backend: https://github.com/HTA-bot/htaintel-backend/blob/main/CLAUDE.md

---

## This repo

Single-file frontend: `index.html` (~200kb)
Live URL: https://htaintel-frontend.vercel.app
Backend: https://htaintel-backend.onrender.com

---

## Key JS functions

| Function               | Purpose                                              |
|------------------------|------------------------------------------------------|
| `fetchAndRenderFeed()` | Main fetch — builds URL from active filters, calls backend |
| `renderFeed(data, q)`  | Renders feed cards as clickable `<a>` tags           |
| `pingBackend()`        | Keep-alive ping every 10 min (Render cold start prevention) |
| `fetchSuggestions(q)`  | Search autocomplete via `/feed?q=`                   |
| `applyFeedFilters()`   | Client-side filter fallback when backend disabled    |

---

## Feed card structure

Cards render as `<a href="${d.link}" target="_blank" rel="noopener noreferrer"
class="feed-card">` — clicking opens the source document in a new tab.
CSS: `display:block; text-decoration:none; color:inherit`.

---

## Backend URL params wired in fetchAndRenderFeed

```js
let url = BACKEND_URL + '/feed?limit=200&sort=' + currentSort;
if (region)                url += '&region=' + region;
if (currentGroupFilter === 'irp') url += '&irp=true';
if (tag && tag !== 'irp')  url += '&tag=' + tag;
if (q)                     url += '&q=' + encodeURIComponent(q);
```

---

## Head tags (all live)

- `<meta name="description">` — SEO description
- `<meta name="robots" content="index, follow">`
- `<link rel="canonical">`
- OG tags: `og:title`, `og:description`, `og:image`, `og:type`, `og:url`
- Twitter card: `summary_large_image`
- Favicon: inline SVG data URI (no separate file needed)
- Note: `og:image` points to `/og-image.png` which does not exist yet

---

## Pending frontend work

- [ ] Create og-image.png (1200x630) and deploy to Vercel
- [ ] Mobile layout audit — test at 375px, 390px, 414px widths
- [ ] Infinite scroll / load-more (backend offset param is ready)
- [ ] Auth / paywall UI (Supabase login flow)
- [ ] Star/bookmark persistence (currently client-side only, resets on refresh)
