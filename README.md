# 💻 PC Deal Tracker // NERD MODE

A live PC parts deal tracker powered by [Slickdeals.net](https://slickdeals.net) and AI — real prices, real links, real product images, and an integrated AI assistant that searches the web live, explains deals, advises on builds, and summarizes the feed. No fake data. No hallucinations.

![Nerd Mode](https://img.shields.io/badge/mode-NERD-00ff41?style=for-the-badge&labelColor=020c02)
![Live Data](https://img.shields.io/badge/data-LIVE-00ff41?style=for-the-badge&labelColor=020c02)
![AI Powered](https://img.shields.io/badge/AI-ENABLED-cc44ff?style=for-the-badge&labelColor=020c02)
![Web Search](https://img.shields.io/badge/web_search-LIVE-00ffcc?style=for-the-badge&labelColor=020c02)
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
- **🤖 Build Advisor + Live Web Search** — type your goal ("best GPU under $400" or "1440p gaming PC under $900") and the AI searches Newegg, Best Buy, Amazon, Microcenter, and price trackers in real time — then combines what it finds with our live deal feed to give you a specific, sourced recommendation with real prices and store links

### Other
- **Works locally AND on GitHub Pages** — no server required
- **Skippable CRT boot sequence** — because nerds deserve nice things 🤓

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

The first time you click an AI feature (Analyze, Build Advisor, or the auto-summary), Puter.js will prompt you to sign in with a free Puter account. Takes about 30 seconds and only happens once. After that, all AI features work seamlessly — no API key, no billing, no setup.

---

## 🤖 How the AI Works

### AI Digest
Runs automatically after the deal feed loads. Reads the current deals and writes a 2-3 sentence plain-English summary: what's hot right now, which categories are active, any standout items worth clicking.

### Deal Analyzer (per card)
Every deal card has an **🤖 ANALYZE** button. Click it and the AI:
- Explains the product in plain English (no jargon)
- Assesses whether the price is competitive
- Flags gotchas: mail-in rebates, open box conditions, limited-time pricing
- Gives a clear verdict: **BUY**, **WAIT**, or **SKIP**

### Build Advisor + Web Search
The **🤖 BUILD ADVISOR** panel (purple button in the sort bar) takes a natural language goal and searches the internet live. While it works you'll see it cycle through status messages in real time:

```
> Searching Newegg for current deals...
> Checking Best Buy sale prices...
> Scanning Amazon listings...
> Cross-referencing price trackers...
```

It combines what it finds on the web with our live Slickdeals feed, then returns specific product recommendations with real prices, real store links, and honest caveats. Typical response time: 8–20 seconds depending on query complexity.

**Example queries that work well:**
- `best GPU under $400 right now`
- `1440p gaming PC under $900, prebuilt or parts`
- `cheapest RTX 5060 deal today`
- `is the RTX 4070 Super a good buy right now`
- `SSD deals at Newegg or Microcenter`

---

## 🏗️ How It Works

This is a **single-file HTML app** — no frameworks, no build tools, no backend.

```
index.html
├── CSS              — Terminal/CRT aesthetic, scanlines, green phosphor glow
├── Boot screen      — Skippable fake BIOS sequence on load
├── Fetch logic      — Slickdeals RSS → rss2json.com → parsed JSON with CORS headers
├── Store detect     — Scans post title + description to identify retailer
├── Category         — Regex-based component classification
├── Price extract    — Pulls $ amounts from deal titles
├── AI Digest        — Puter.js summarizes the feed on load
├── AI Analyzer      — Puter.js explains individual deals on demand
├── Build Advisor    — Puter.js + live web_search tool finds current deals across the web
└── Render           — Filter, sort, search, display with thumbnails
```

### Why Slickdeals?

Reddit's API (`r/buildapcsales`) was the original plan, but Reddit locked it down in 2024-2025 — all requests return 403 regardless of proxies or headers. Slickdeals covers every major PC retailer (Amazon, Best Buy, Newegg, Walmart, Microcenter, Dell, Alienware, and more), posts real prices, and provides a public RSS feed.

### Why rss2json.com?

Slickdeals provides RSS but not a JSON API with CORS headers. [rss2json.com](https://api.rss2json.com) converts any public RSS feed to JSON served with proper CORS headers — so the app works from any origin, local file or hosted domain, with no backend required.

### Why Puter.js?

[Puter.js](https://js.puter.com/v2/) provides free access to AI models (GPT, Claude, Gemini, and others) directly from frontend code with no API key and no backend. It uses a "user-pays" model where each user's free Puter account covers their own AI usage. For personal use this is effectively unlimited and free.

The Build Advisor uses Puter's `web_search` tool integration, which lets the AI search the open web in real time during a query — the same capability that powers the live price lookups at Newegg, Best Buy, and Amazon.

---

## 🛠️ Customization

Everything lives in one file — `index.html`.

| What to change | Where to find it |
|---|---|
| Color scheme | `:root` CSS variables at the top |
| Boot messages | `bootLines` array in the `<script>` |
| Search queries (feed) | `SEARCH_QUERIES` array |
| Category filters | `detectCategory()` function |
| Store detection | `detectStore()` function |
| Feed refresh interval | `setInterval(fetchDeals, 5 * 60 * 1000)` |
| AI model | `model: 'gpt-5.4-nano'` in each `puter.ai.chat()` call |
| Web search (Build Advisor) | `tools: [{ type: 'web_search' }]` in `runBuildAdvisor()` |

---

## 📁 Files

```
pc-deals/
├── README.md
├── index.html       ← main app
└── test.html        ← Puter.js web search test page (can delete after testing)
```

---

## 🤓 Built With

- Vanilla HTML, CSS, JavaScript — zero frameworks
- [Slickdeals RSS](https://slickdeals.net) — public deal feed, community verified
- [rss2json.com](https://api.rss2json.com) — RSS to JSON with CORS headers
- [Puter.js](https://js.puter.com/v2/) — free keyless AI with live web search (GPT-5.4-nano)
- [Orbitron](https://fonts.google.com/specimen/Orbitron) + [Share Tech Mono](https://fonts.google.com/specimen/Share+Tech+Mono) — Google Fonts
- Terminal green CRT aesthetic — because nerds deserve nice things

---

*Built with ❤️ by Dad*
