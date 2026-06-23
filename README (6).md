# 💻 PC Deal Tracker // NERD MODE

A live PC parts deal tracker that pulls real, community-verified deals from [r/buildapcsales](https://reddit.com/r/buildapcsales) — no fake prices, no hallucinated links.

![Nerd Mode](https://img.shields.io/badge/mode-NERD-00ff41?style=for-the-badge&labelColor=020c02)
![Live Data](https://img.shields.io/badge/data-LIVE-00ff41?style=for-the-badge&labelColor=020c02)
![No BS](https://img.shields.io/badge/fake_prices-NONE-ff3333?style=for-the-badge&labelColor=020c02)

---

## 🔥 Features

- **Real live data** from r/buildapcsales — community members post and verify deals from all major retailers
- **Store detection** — automatically identifies Amazon, Best Buy, Newegg, Walmart, Microcenter, Dell, HP, Alienware, and more
- **Filter by category** — GPU, CPU, RAM, SSD, Prebuilt, Monitor, Laptop, Cooling, PSU/Case, Motherboard, Peripherals
- **Sort by** Hottest, Newest, or Most Discussed
- **Search bar** — hunt for specific parts instantly
- **🔥 HOT badge** on anything with 500+ community upvotes
- **Auto-refreshes** every 5 minutes — stays current without reloading
- **CORS proxy fallback chain** — works locally AND on GitHub Pages
- **Boot sequence** — because why not

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

Just open `pc-deal-tracker.html` in your browser. The app will automatically route through a CORS proxy to fetch live Reddit data even from a local file.

---

## 🏗️ How It Works

This is a **single-file HTML app** — no build step, no dependencies, no server required.

```
pc-deal-tracker.html
├── CSS          — Terminal/CRT aesthetic, scanlines, green phosphor glow
├── Boot screen  — Fake BIOS sequence on load
├── Fetch logic  — Direct Reddit API → proxy fallbacks if needed
├── Store detect — Scans post title + URL to identify retailer
├── Category     — Regex-based component classification
└── Render       — Filter, sort, search, display
```

### Why r/buildapcsales?

Most major retailers (Amazon, Best Buy, Newegg, etc.) don't offer free public APIs for deal/price data, and they actively block scrapers. r/buildapcsales is a community of PC builders who manually post and verify deals from all those stores — it's essentially a free, human-curated deal feed covering every retailer on the list.

### CORS Proxy Chain

When running locally (`file://`), browsers block cross-origin requests. The app tries four methods in order:

1. **Direct fetch** — works on GitHub Pages / any hosted domain
2. **corsproxy.io** — free proxy fallback
3. **allorigins.win** — secondary fallback  
4. **cors-anywhere** — last resort

On GitHub Pages, method #1 succeeds and proxies are never used.

---

## 🛠️ Customization

Want to tweak it? Everything is in one file — `pc-deal-tracker.html`.

| What to change | Where to find it |
|---|---|
| Color scheme | `:root` CSS variables at the top |
| Boot messages | `bootLines` array in the `<script>` |
| Category filters | `detectCategory()` function |
| Store detection | `detectStore()` function |
| Refresh interval | `setInterval(fetchDeals, 5 * 60 * 1000)` |
| Hot deal threshold | `d.score >= 500` in `renderDeals()` |

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
- [Reddit JSON API](https://www.reddit.com/r/buildapcsales/hot.json) — public, no key required
- [Orbitron](https://fonts.google.com/specimen/Orbitron) + [Share Tech Mono](https://fonts.google.com/specimen/Share+Tech+Mono) — Google Fonts
- Terminal green aesthetic — because nerds deserve nice things

---

*Built with ❤️ by Dad*
