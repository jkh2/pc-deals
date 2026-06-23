# 💻 PC Deal Tracker // NERD MODE

A live PC parts deal tracker pulling real, community-verified deals from [Slickdeals.net](https://slickdeals.net) — real prices, real links, real product images. No fake data. No hallucinations.

![Nerd Mode](https://img.shields.io/badge/mode-NERD-00ff41?style=for-the-badge&labelColor=020c02)
![Live Data](https://img.shields.io/badge/data-LIVE-00ff41?style=for-the-badge&labelColor=020c02)
![No BS](https://img.shields.io/badge/fake_prices-NONE-ff3333?style=for-the-badge&labelColor=020c02)

---

## 🔥 Features

- **Real live data** from Slickdeals.net — community members post and verify deals from all major retailers
- **Product thumbnails** — actual images pulled from each deal listing
- **Price extraction** — dollar amounts pulled directly from deal titles and displayed prominently
- **Store detection** — automatically identifies Amazon, Best Buy, Newegg, Walmart, Microcenter, Costco, Dell, HP, Alienware, and more
- **Filter by category** — GPU, CPU, RAM, SSD, Prebuilt, Monitor, Laptop, Cooling, PSU/Case, Motherboard, Peripherals
- **Sort by** Newest or Lowest Price
- **Search bar** — hunt for specific parts instantly
- **Auto-refreshes** every 5 minutes — stays current without reloading
- **Works locally AND on GitHub Pages** — no server required
- **CRT boot sequence** — because why not 🤓

---

## 🚀 Live Demo

👉 [https://jkh2.github.io/pc-deals](https://jkh2.github.io/pc-deals)

---

## 📦 How to Use

### Option 1 — GitHub Pages (Recommended)

1. Fork this repo
2. Go to **Settings → Pages**
3. Set source to **Deploy from a branch → main → / (root)**
4. Visit `https://YOUR-USERNAME.github.io/pc-deals`

### Option 2 — Run Locally

Just open `pc-deal-tracker.html` in your browser. No build step, no server, no dependencies.

---

## 🏗️ How It Works

This is a **single-file HTML app** — no frameworks, no build tools, no backend.

```
pc-deal-tracker.html
├── CSS            — Terminal/CRT aesthetic, scanlines, green phosphor glow
├── Boot screen    — Fake BIOS sequence on load
├── Fetch logic    — Slickdeals RSS → rss2json.com → parsed JSON with CORS headers
├── Store detect   — Scans post title + description to identify retailer
├── Category       — Regex-based component classification
├── Price extract  — Pulls $ amounts from deal titles
└── Render         — Filter, sort, search, display with thumbnails
```

### Why Slickdeals?

Reddit's API (`r/buildapcsales`) was the original data source, but Reddit locked it down in 2024-2025 — all requests return 403 regardless of proxies or headers. Slickdeals is actually a better fit: it's a dedicated deal community that covers every major PC retailer (Amazon, Best Buy, Newegg, Walmart, Microcenter, Dell, Alienware, and more), posts real prices, and provides a public RSS feed.

### Why rss2json.com?

Slickdeals provides an RSS feed but not a JSON API with CORS headers. [rss2json.com](https://api.rss2json.com) converts any public RSS feed to JSON and serves it with proper CORS headers, so the app works from any origin — local file or hosted domain — with no backend required. The free tier is sufficient for this use case.

---

## 🛠️ Customization

Everything lives in one file — `pc-deal-tracker.html`.

| What to change | Where to find it |
|---|---|
| Color scheme | `:root` CSS variables at the top |
| Boot messages | `bootLines` array in the `<script>` |
| Search queries | `SEARCH_QUERIES` array |
| Category filters | `detectCategory()` function |
| Store detection | `detectStore()` function |
| Refresh interval | `setInterval(fetchDeals, 5 * 60 * 1000)` |

---

## 📁 Files

```
pc-deals/
├── README.md
└── pc-deal-tracker.html
```

---

## 🤓 Built With

- Vanilla HTML, CSS, JavaScript — zero frameworks
- [Slickdeals RSS](https://slickdeals.net) — public deal feed, community verified
- [rss2json.com](https://api.rss2json.com) — RSS to JSON with CORS headers
- [Orbitron](https://fonts.google.com/specimen/Orbitron) + [Share Tech Mono](https://fonts.google.com/specimen/Share+Tech+Mono) — Google Fonts
- Terminal green CRT aesthetic — because nerds deserve nice things

---

*Built with ❤️ by Dad*
