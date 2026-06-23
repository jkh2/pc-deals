# 💻 PC Deal Tracker // NERD MODE

A live PC parts deal tracker powered by [Slickdeals.net](https://slickdeals.net) and AI — real prices, real links, real product images, and an integrated AI assistant that explains deals, advises on builds, and summarizes the feed. No fake data. No hallucinations.

![Nerd Mode](https://img.shields.io/badge/mode-NERD-00ff41?style=for-the-badge&labelColor=020c02)
![Live Data](https://img.shields.io/badge/data-LIVE-00ff41?style=for-the-badge&labelColor=020c02)
![AI Powered](https://img.shields.io/badge/AI-ENABLED-cc44ff?style=for-the-badge&labelColor=020c02)
![No BS](https://img.shields.io/badge/fake_prices-NONE-ff3333?style=for-the-badge&labelColor=020c02)

---

## 🔥 Features

### Deal Tracking
- **Real live data** from Slickdeals.net — community-verified deals from all major retailers
- **Product thumbnails** — actual images pulled from each deal listing
- **Price extraction** — dollar amounts pulled from deal titles and displayed prominently
- **Store detection** — automatically identifies Amazon, Best Buy, Newegg, Walmart, Microcenter, Costco, Dell, HP, Alienware, and more
- **Filter by category** — GPU, CPU, RAM, SSD, Prebuilt, Monitor, Laptop, Cooling, PSU/Case, Motherboard, Peripherals
- **Sort by** Newest or Lowest Price
- **Search bar** — hunt for specific parts instantly
- **Auto-refreshes** every 5 minutes

### AI Features (powered by Puter.js — free, no API key)
- **🤖 AI Digest** — auto-generated feed summary on every load: what's hot, which categories are active, standout deals
- **🤖 Analyze button** — per-card AI analysis: plain English explanation of the product, price assessment, gotcha flags (rebates, open box), and a BUY / WAIT / SKIP verdict
- **🤖 Build Advisor** — type your goal ("1440p gaming PC under $900") and the AI scans the live deal list and tells you which specific deals match and why

### Other
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

The main file is `index.html` — GitHub Pages will serve it automatically at the root URL.

### Option 2 — Run Locally

Just open `index.html` in your browser. No build step, no server, no dependencies.

### AI Setup (one time)

The first time you click an AI feature (Analyze, Build Advisor, or the auto-summary), Puter.js will prompt you to sign in with a free Puter account. This takes about 30 seconds and only happens once. After that, all AI features work seamlessly with no API key required.

---

## 🏗️ How It Works

This is a **single-file HTML app** — no frameworks, no build tools, no backend.

```
index.html
├── CSS            — Terminal/CRT aesthetic, scanlines, green phosphor glow
├── Boot screen    — Fake BIOS sequence on load (skippable)
├── Fetch logic    — Slickdeals RSS → rss2json.com → parsed JSON with CORS headers
├── Store detect   — Scans post title + description to identify retailer
├── Category       — Regex-based component classification
├── Price extract  — Pulls $ amounts from deal titles
├── AI Digest      — Puter.js summarizes the feed on load
├── AI Analyzer    — Puter.js explains individual deals on demand
├── Build Advisor  — Puter.js matches live deals to your build goal
└── Render         — Filter, sort, search, display with thumbnails
```

### Why Slickdeals?

Reddit's API (`r/buildapcsales`) was the original data source, but Reddit locked it down — all requests return 403 regardless of proxies or headers. Slickdeals covers every major PC retailer (Amazon, Best Buy, Newegg, Walmart, Microcenter, Dell, Alienware, and more), posts real prices, and provides a public RSS feed.

### Why rss2json.com?

Slickdeals provides RSS but not a JSON API with CORS headers. [rss2json.com](https://api.rss2json.com) converts any public RSS feed to JSON and serves it with proper CORS headers, so the app works from any origin — local file or hosted domain — with no backend required.

### Why Puter.js?

[Puter.js](https://js.puter.com/v2/) provides free access to AI models (GPT, Claude, Gemini, and others) directly from frontend code with no API key and no backend. It uses a "user-pays" model where each user's free Puter account covers their own AI usage. For a personal app this is effectively unlimited and free.

---

## 🛠️ Customization

Everything lives in one file — `index.html`.

| What to change | Where to find it |
|---|---|
| Color scheme | `:root` CSS variables at the top |
| Boot messages | `bootLines` array in the `<script>` |
| Search queries | `SEARCH_QUERIES` array |
| Category filters | `detectCategory()` function |
| Store detection | `detectStore()` function |
| Refresh interval | `setInterval(fetchDeals, 5 * 60 * 1000)` |
| AI model | `model: 'gpt-5.4-nano'` in each `puter.ai.chat()` call |

---

## 📁 Files

```
pc-deals/
├── README.md
└── index.html
```

---

## 🤓 Built With

- Vanilla HTML, CSS, JavaScript — zero frameworks
- [Slickdeals RSS](https://slickdeals.net) — public deal feed, community verified
- [rss2json.com](https://api.rss2json.com) — RSS to JSON with CORS headers
- [Puter.js](https://js.puter.com/v2/) — free keyless AI (GPT-5.4-nano)
- [Orbitron](https://fonts.google.com/specimen/Orbitron) + [Share Tech Mono](https://fonts.google.com/specimen/Share+Tech+Mono) — Google Fonts
- Terminal green CRT aesthetic — because nerds deserve nice things

---

*Built with ❤️ by Dad*
