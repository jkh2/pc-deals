# 💻 PC Deal Tracker // NERD MODE

A live PC parts and tech deal tracker powered by [Slickdeals.net](https://slickdeals.net) and AI — real prices, real links, real product images, and an integrated AI assistant that searches the web live, explains deals, advises on builds, and summarizes the feed. No fake data. No hallucinations. No pistachios.

![Nerd Mode](https://img.shields.io/badge/mode-NERD-00ff41?style=for-the-badge&labelColor=020c02)
![Live Data](https://img.shields.io/badge/data-LIVE-00ff41?style=for-the-badge&labelColor=020c02)
![AI Powered](https://img.shields.io/badge/AI-ENABLED-cc44ff?style=for-the-badge&labelColor=020c02)
![Web Search](https://img.shields.io/badge/web_search-LIVE-00ffcc?style=for-the-badge&labelColor=020c02)
![No BS](https://img.shields.io/badge/fake_prices-NONE-ff3333?style=for-the-badge&labelColor=020c02)

---

## 🔥 Features

### Deal Tracking
- **Real live data** from Slickdeals.net — community-verified deals from all major retailers
- **16 targeted feed queries** — each proven to return real tech deals, fired sequentially to avoid rate limits
- **Three-tier tech filter** — brands, specs, and category keywords working together to let tech through and block everything else
- **Product thumbnails** — extracted from deal content, not just the RSS thumbnail field
- **Price extraction** — dollar amounts pulled from deal titles and displayed prominently
- **Store detection** — automatically identifies Amazon, Best Buy, Newegg, Walmart, Microcenter, Costco, Dell, HP, Alienware, and more
- **17 category filters** — GPU, CPU, RAM, SSD, Prebuilt, Monitor, Laptop, Cooling, PSU/Case, Motherboard, Peripherals, Audio & Video, Networking, Phones & Tablets, Smart Home, Gadgets
- **Category chip on every card** — shows which category each deal landed in at a glance
- **Sort by** Newest or Lowest Price
- **Search bar** — hunt for specific parts instantly
- **Auto-refreshes** every 5 minutes

### AI Features (powered by Puter.js — free, no API key)
- **🤖 AI Digest** — auto-generated feed summary on first load: what's hot, which categories are active, standout deals
- **🤖 Analyze button** — per-card AI analysis: plain English explanation of the product, price assessment, gotcha flags (rebates, open box), and a BUY / WAIT / SKIP verdict
- **🤖 Build Advisor + Live Web Search** — type your goal and the AI searches Newegg, Best Buy, Amazon, Microcenter, and price trackers in real time, then combines live web results with the current deal feed for specific, sourced recommendations with a CLEAR / ASK ANOTHER button

### UX Polish
- **Skippable CRT boot sequence** — click anywhere or press any key to skip
- **Night-vision nerd mascot** — appears on boot screen and all AI loading states with speech bubble and cycling status messages
- **Refresh fail toast** — background refresh failures show a small notification without wiping visible cards
- **Mobile-friendly** — filter bar scrolls with fade hint, nerd image hides on small screens

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

The main file is `index.html` — GitHub Pages serves it automatically at the root URL.

### Option 2 — Run Locally

Just open `index.html` in your browser. No build step, no server, no dependencies.

### AI Setup (one time only)

The first time you click an AI feature, Puter.js will prompt you to sign in with a free Puter account. Takes about 30 seconds and only happens once. After that, all AI features work seamlessly — no API key, no billing, no setup.

---

## 🤖 How the AI Works

### AI Digest
Runs automatically after the deal feed loads on first visit only (not on every refresh). Reads the current deals and writes a 2-3 sentence plain-English summary of what's hot right now.

### Deal Analyzer (per card)
Every deal card has an **🤖 ANALYZE** button. Click it and the AI explains the product in plain English, assesses the price, flags gotchas (mail-in rebates, open box, limited time), and gives a **BUY / WAIT / SKIP** verdict.

### Build Advisor + Web Search
The **🤖 BUILD ADVISOR** panel (purple button in the sort bar) takes a natural language goal and searches the internet live. While it works, the nerd mascot cycles through status messages in real time. It combines live web results with the current deal feed and returns specific product recommendations with real prices and store links.

**Example queries:**
- `best GPU under $400 right now`
- `1440p gaming PC under $900, prebuilt or parts`
- `cheapest RTX 5060 deal today`
- `best wifi router under $150`
- `good drone for under $300`
- `SSD deals at Newegg or Microcenter`

---

## 🏗️ How It Works

This is a **single-file HTML app** — no frameworks, no build tools, no backend.

```
index.html
├── CSS              — Terminal/CRT aesthetic, scanlines, green phosphor glow, animations
├── Boot screen      — Skippable fake BIOS sequence with night-vision nerd mascot
├── Fetch logic      — 16 targeted queries → Slickdeals RSS → rss2json.com → JSON
│                      Fired sequentially with 300ms delays to avoid rate limiting
├── Three-tier filter — Brands / Specs / Categories — must match at least one tier
├── Thumbnail extract — Pulled from RSS content field, not just thumbnail field
├── Store detect     — Scans title + description + content to identify retailer
├── Category detect  — 17-bucket regex classification with priority ordering
├── Price extract    — Pulls real $ amounts, filters out coupon/discount amounts
├── AI Digest        — Puter.js summarizes the feed on first load only
├── AI Analyzer      — Puter.js explains individual deals on demand
├── Build Advisor    — Puter.js + live web_search finds current deals across the web
└── Render           — Filter, sort, search, display with thumbnails and category chips
```

### Why Slickdeals?
Reddit's API (`r/buildapcsales`) was the original plan, but Reddit locked it down — all requests return 403. Slickdeals covers every major PC retailer, posts real prices, and provides a public RSS feed.

### Why sequential fetching?
We fire 16 queries to Slickdeals' RSS feed to cover all tech categories. Firing them all simultaneously with `Promise.all` hits rss2json's rate limiter, silently failing half the queries and leaving categories empty. Sequential fetching with 300ms delays between requests gets all 16 feeds reliably.

### The three-tier filter
The tech relevance filter uses three independent regex sets. A deal passes if it matches **any one** of them:

| Tier | What it matches | Examples |
|------|----------------|---------|
| **Brands** | Companies that primarily make tech | Nvidia, Corsair, Logitech, DJI, Anker, Lenovo, Bose... |
| **Specs** | Technical specifications unique to tech | RTX, DDR5, NVMe, 1440p, 144hz, WiFi 6, USB-C... |
| **Categories** | Specific tech product compound nouns | "gaming laptop", "cpu cooler", "wireless router", "3d printer"... |

Single generic words (`cooler`, `display`, `memory`, `case`) are never used alone — only in compound phrases that can't false-positive on non-tech products.

### Why rss2json.com?
Slickdeals provides RSS but not a JSON API with CORS headers. rss2json.com converts any public RSS feed to JSON with proper CORS headers, so the app works from any origin with no backend.

### Why Puter.js?
[Puter.js](https://js.puter.com/v2/) provides free access to AI models directly from frontend code — no API key, no backend. The Build Advisor uses Puter's `web_search` tool to search the open web in real time.

---

## 🛠️ Customization

Everything lives in one file — `index.html`.

| What to change | Where to find it |
|---|---|
| Color scheme | `:root` CSS variables at the top |
| Boot messages | `bootLines` array in the `<script>` |
| Feed queries | `SEARCH_QUERIES` array |
| Brand filter | `_BRANDS` regex |
| Spec filter | `_SPECS` regex |
| Category filter | `_CATS` regex |
| Category detection | `detectCategory()` function |
| Category labels on cards | `catLabel` object |
| Store detection | `detectStore()` function |
| Feed refresh interval | `setInterval(fetchDeals, 5 * 60 * 1000)` |
| Request delay | `setTimeout(r, 300)` between queries |
| AI model | `model: 'gpt-5.4-nano'` in each `puter.ai.chat()` call |
| Web search | `tools: [{ type: 'web_search' }]` in `runBuildAdvisor()` |

---

## 📁 Files

```
pc-deals/
├── README.md
├── index.html       ← main app (single file, everything included)
└── nerdy.png        ← night-vision nerd mascot (STATUS: AWKWARD. MISSION: BE IMPRESSIVE.)
```

---

## 🤓 Built With

- Vanilla HTML, CSS, JavaScript — zero frameworks
- [Slickdeals RSS](https://slickdeals.net) — public deal feed, community verified
- [rss2json.com](https://api.rss2json.com) — RSS to JSON with CORS headers
- [Puter.js](https://js.puter.com/v2/) — free keyless AI with live web search (GPT-5.4-nano)
- [Orbitron](https://fonts.google.com/specimen/Orbitron) + [Share Tech Mono](https://fonts.google.com/specimen/Share+Tech+Mono) — Google Fonts
- Night-vision nerd mascot — STATUS: AWKWARD. MISSION: BE IMPRESSIVE.
- Terminal green CRT aesthetic — because nerds deserve nice things

---

*Built with ❤️ by Dad*
