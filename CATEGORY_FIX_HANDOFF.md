# PC Deals Category Fix Handoff

This file documents the safe, targeted category fixes recommended for `index.html`.

A backup branch was created before any site-affecting work:

```txt
backup-before-category-fix-20260624
```

No live site file was changed by this handoff.

## Problem summary

The category buttons in `index.html` use exact `data-filter` slugs such as:

- `gpu`
- `cpu`
- `ram`
- `ssd`
- `prebuilt`
- `monitor`
- `laptop`
- `cooling`
- `psu`
- `mobo`
- `peripheral`
- `audio`
- `networking`
- `mobile`
- `smarthome`
- `gadgets`

The render function filters by exact category match:

```js
if (activeFilter !== 'all') deals = deals.filter(d => d.category === activeFilter);
```

So the buttons and render logic are basically fine. The weak point is category assignment before rendering.

## Fix 1: categorize using the combined text

Inside `fetchDeals()`, the code builds a richer text string:

```js
const combined = rawTitle + ' ' + (item.description || '') + ' ' + (item.content || '');
```

But it currently categorizes using only the title:

```js
category: detectCategory(rawTitle),
```

Change that to:

```js
category: detectCategory(combined),
```

This lets category detection use the title, description, and content from Slickdeals.

## Fix 2: replace `detectCategory()` with priority-based detection

Whole devices need to be detected before individual parts. A gaming laptop or prebuilt desktop often contains words like RTX, RAM, SSD, Ryzen, or Intel. Those part keywords should not steal the deal away from `laptop` or `prebuilt`.

Recommended priority order:

1. Laptop
2. Prebuilt / desktop PC / mini PC
3. Mobile / tablets / wearables
4. Audio & video
5. Monitor
6. Smart home / streaming
7. Networking
8. Cooling
9. Motherboard
10. PSU / case
11. GPU
12. CPU
13. RAM
14. SSD / storage
15. Peripherals
16. Gadgets
17. Other

Suggested replacement:

```js
function detectCategory(text) {
  const t = (text || '').toLowerCase();

  // 1. Full machines first. These often contain GPU/RAM/SSD/CPU words.
  if (/\b(laptop|notebook|chromebook|macbook|ultrabook|thinkpad|ideapad|zenbook|vivobook|gaming laptop|helios|predator laptop|katana|raider \d|zephyrus|strix scar|rog flow|rog strix g|swift x|nitro \d|aspire \d|inspiron \d|pavilion \d|envy \d|spectre x|yoga \d|flex \d)\b/.test(t)) {
    return 'laptop';
  }

  if (/\b(prebuilt|pre-built|pre built|desktop pc|gaming pc|gaming desktop|desktop computer|mini pc|nuc|workstation|cyberpower|ibuypower|skytech|alienware aurora|omen desktop|legion tower)\b/.test(t)) {
    return 'prebuilt';
  }

  // 2. Phones, tablets, wearables
  if (/\b(smartphone|iphone|pixel \d|galaxy s|galaxy z|android phone|tablet|ipad|drawing tablet|wacom|smartwatch|apple watch|galaxy watch|fitness tracker|wearable)\b/.test(t)) {
    return 'mobile';
  }

  // 3. Audio / video before monitor, so TVs do not get stolen by "4K".
  if (/\b(tv|smart tv|oled tv|qled tv|soundbar|speaker|subwoofer|headphone|headphones|headset|earbuds|earphones|airpods|microphone|condenser mic|audio interface|dac\b|amplifier|\bamp\b|projector|webcam|capture card|stream deck|action cam|gopro|dashcam|camera)\b/.test(t)) {
    return 'audio';
  }

  // 4. Monitor/display
  if (/\b(monitor|gaming monitor|curved monitor|ultrawide|computer display|pc display|144hz|165hz|240hz|360hz)\b/.test(t)) {
    return 'monitor';
  }

  // 5. Smart home / streaming
  if (/\b(fire tv|fire stick|roku|apple tv|chromecast|streaming stick|android tv|smart plug|smart bulb|smart switch|smart home|alexa|echo dot|echo show|google home|ring doorbell|ring camera|smart lock|smart thermostat|nest)\b/.test(t)) {
    return 'smarthome';
  }

  // 6. Networking
  if (/\b(router|wi-fi|wifi|mesh network|mesh wifi|access point|modem|ethernet|network switch|powerline|range extender)\b/.test(t)) {
    return 'networking';
  }

  // 7. PC cooling before CPU
  if (/\b(cpu cooler|pc cooler|cpu fan|aio cooler|liquid cooler|water cooler|air cooler|heatsink|thermal paste|noctua|be quiet|arctic freezer|deepcool|scythe)\b/.test(t)) {
    return 'cooling';
  }

  // 8. Motherboards before CPU/RAM/GPU, but not before full systems
  if (/\b(motherboard|mobo\b|\bmb\b|aorus elite|aorus pro|aorus master|prime b\d|tuf gaming b\d|rog strix b\d|rog maximus|pro b\d|msi pro b|msi mag b|b\d{3}[a-z]?[-\s]|x\d{3}[a-z]?[-\s]|z\d{3}[a-z]?[-\s])\b/.test(t)) {
    return 'mobo';
  }

  // 9. PSU / case
  if (/\b(psu|power supply|\bcase\b|chassis|atx case|itx case|matx|tower case|mid tower|full tower|mini itx case)\b/.test(t)) {
    return 'psu';
  }

  // 10. Individual PC parts
  if (/\b(rtx|gtx|rx \d|arc |radeon|geforce|gpu|graphics card|video card)\b/.test(t)) {
    return 'gpu';
  }

  if (/\b(ryzen|intel core|core i[3579]|i[3579]-\d|cpu|processor|xeon|threadripper)\b/.test(t)) {
    return 'cpu';
  }

  if (/\b(ddr[3-5]|\bram\b|memory kit|dimm|sodimm|lpddr[45])\b/.test(t)) {
    return 'ram';
  }

  if (/\b(ssd|nvme|hdd|hard drive|m\.2|solid state|storage|flash drive|thumb drive|usb drive|sd card|microsd|external ssd|external hdd)\b/.test(t)) {
    return 'ssd';
  }

  // 11. Peripherals
  if (/\b(keyboard|mouse\b|mousepad|mouse pad|trackpad|trackball|controller|gamepad|joystick|racing wheel|flight stick)\b/.test(t)) {
    return 'peripheral';
  }

  // 12. Gadgets / maker gear
  if (/\b(vr headset|oculus|quest \d|meta quest|vision pro|drone|quadcopter|raspberry pi|arduino|fpga|dev board|3d printer|filament|resin printer|oscilloscope|multimeter|soldering|power bank|wireless charger|gan charger|usb hub|docking station|kvm|egpu|external gpu|scanner|label maker|e-reader|kindle|robot|robotic|stem kit|electronics kit)\b/.test(t)) {
    return 'gadgets';
  }

  return 'other';
}
```

## Fix 3: add missing Slickdeals search queries

The current `SEARCH_QUERIES` list does not directly ask for some categories, especially CPU, RAM, PSU, and cases. Add these near the existing queries:

```js
'desktop+cpu+processor',
'ryzen+cpu',
'intel+cpu',
'ddr5+ram',
'computer+memory',
'pc+power+supply',
'psu+power+supply',
'pc+case',
'gaming+pc+case',
```

## Test cases

After patching, these should classify correctly:

- `RTX 4060 gaming laptop` -> `laptop`, not `gpu`
- `Gaming desktop with RTX 4070, 32GB RAM, 1TB SSD` -> `prebuilt`, not `gpu`, `ram`, or `ssd`
- `Ryzen 7 CPU` -> `cpu`
- `DDR5 memory kit` -> `ram`
- `NVMe SSD` -> `ssd`
- `750W PSU` -> `psu`
- `B650 motherboard bundle` -> `mobo`
- `CPU cooler / AIO` -> `cooling`

## Recommended safe workflow

1. Create a feature branch from `main`.
2. Apply only the changes above to `index.html`.
3. Open the site locally or in GitHub Pages preview if available.
4. Click each category filter and verify cards appear under reasonable categories.
5. Merge only after visual verification.
