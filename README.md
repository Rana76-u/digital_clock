# Bangladesh Clock

A single-file, full-screen web clock for a wall display, TV or desk monitor. It shows the current Dhaka time in very large type, the five daily prayer times in Bangla, and two other cities' local times, all on a background that changes with the time of day.

No build step, no framework, no server. Open the HTML file in a browser.

## Features

- **Big Dhaka clock.** Hours and minutes in heavy Sora numerals that fill the full width of the main column, always with a leading zero (02:39). Seconds and AM/PM sit above the clock on the right, with the full date and a live seconds bar.
- **Five prayer times, Bangla first.** ফজর, যোহর, আসর, মাগরিব and এশা are the main labels, with English underneath.
  - The prayer running now is a solid filled card ("এখন চলছে") with a progress bar.
  - The next prayer has a glowing outline and a live countdown ("পরবর্তী · বাকি hh:mm:ss").
  - Prayers that have passed are dimmed.
- **Prayer times calculated offline.** No API is needed, so they always show, even with no internet.
- **Two world clocks.** Nova Scotia (Canadian flag) and London (UK flag) by default. Each card is a live sky that blends from night to sunrise, day, sunset and back, with a moving sun or moon, stars, drifting clouds and a small দিন / রাত / ভোর / সন্ধ্যা label.
- **Time-of-day theme.** The whole page shifts through five palettes that follow the real prayer times: dark at dawn, a warm light theme in the morning, a cool light theme at midday, dark at dusk, and pure black at night.
- **Photo backgrounds.** Random landscape photos crossfade every 10 minutes, with a gradient fallback if none can load.
- **Display-friendly extras.** Full-screen toggle, hidden mouse cursor when idle, and the screen is kept awake where the browser allows it.

## Quick start

1. Download `bangladesh-clock-v6.html`. You may want to rename it to `index.html`.
2. Open it in Chrome, Edge, Firefox or Safari (recent versions).
3. Press **F** or double-click the page for full screen.

The page needs the internet only for the Google Fonts and the background photos. If those can't load, it falls back to system fonts and an animated gradient, and the clocks and prayer times keep working.

> Open the file in a normal browser tab. Some chat and preview panes block requests to other websites, so photos and fonts won't load there.

## Controls

| Key or action | What it does |
|---|---|
| **F** or double-click | Toggle full screen |
| **P** | Preview each colour palette in turn (press again to cycle back to live) |
| Mouse idle for 3 seconds | Hides the cursor |

## Configuration

Everything you'd normally change is in the `SETTINGS` block at the top of the `<script>`.

| Setting | Purpose |
|---|---|
| `HOME` | Main time zone (`Asia/Dhaka`) |
| `ZONES` | The two other cities: `name`, `country`, `tz` (IANA time zone), `flag` |
| `LAT`, `LNG`, `UTC_OFFSET` | Location used for prayer-time calculation (Dhaka by default) |
| `FAJR_ANGLE`, `ISHA_ANGLE` | Twilight angles (18° each, the Karachi method) |
| `ASR_FACTOR` | `2` = Hanafi (Bangladesh), `1` = Shafi'i |
| `OFFSETS` | Minutes to add or subtract per prayer to match your local timetable |
| `PHASES` | The five colour palettes. `scrim` is how strongly the palette covers the photo (`1` = solid, no photo) |
| `PEXELS_KEY` | Your Pexels API key (optional, see below) |
| `PEXELS_QUERIES` | Search words for background photos |
| `BG_EVERY_MIN` | Minutes between background changes |

### Changing the world clocks

Edit `ZONES`, for example:

```js
{ name:"Ontario", country:"Canada", tz:"America/Toronto", flag:"ca" }
```

Built-in flags are `ca` (Canada) and `uk` (United Kingdom). For another country, add a small SVG to the `FLAGS` object. The sky on each card follows that city's own local time automatically.

## Prayer times

- Calculated in the page with standard solar-position formulas, for Dhaka (23.8103° N, 90.4125° E, UTC+6).
- Defaults: Karachi method (Fajr and Isha at 18°), Hanafi Asr, Maghrib at sunset, Dhuhr 2 minutes after solar noon.
- Results can differ from a printed mosque or Islamic Foundation timetable by a minute or two. Use `OFFSETS` to line them up, for example `{ Fajr:-1, Dhuhr:0, Asr:1, Maghrib:2, Isha:0 }`.
- The running prayer is the last one that has started. After Isha it stays highlighted through the night until the next Fajr.

## Colour phases

| Phase | Theme | Runs from → to | Background | Time | Secondary | Accent |
|---|---|---|---|---|---|---|
| Dawn | Dark | Fajr → sunrise | `#1C2331` | `#F4C430` | `#A0A0C0` | `#E07A5F` |
| Morning | Light (warm) | Sunrise → Dhuhr | `#FBF1E1` | `#22180C` | `#8A5200` | `#B45F06` |
| Midday | Light (cool) | Dhuhr → Maghrib | `#EEF6F8` | `#0B1B22` | `#0A6A80` | `#0F766E` |
| Dusk | Dark | Maghrib → Isha | `#1A1423` | `#F3E9DC` | `#E07A5F` | `#D97706` |
| Night | Dark | Isha → Fajr | `#000000` | `#D4A373` | `#8A8A8A` | `#B5838D` |

Each entry in `PHASES` also has a `mode` (`"dark"` or `"light"`) and an optional `onacc`, the text colour used on the solid "running now" card.

The night secondary colour is lighter than a typical dark gray so small text stays readable on black from a distance.

## Background photos

The page tries these sources in order:

1. **Pexels** (if `PEXELS_KEY` is set). The best quality, with random landscapes such as mountains, ocean and forest.
2. **Bing daily image.** This is an unofficial endpoint, and browsers may block it, in which case the page moves on.
3. **Picsum.** Random photos from Unsplash, with no key needed.
4. **Animated gradient.** Always shown underneath, and used if no photo can be loaded.

### Getting a free Pexels key

1. Create a free account at [pexels.com](https://www.pexels.com).
2. Request a key at [pexels.com/api/new](https://www.pexels.com/api/new/). It is issued instantly.
3. Paste it into `PEXELS_KEY` in the settings block.

The default limit is 200 requests per hour and 20,000 per month. The page makes about one request every 10 minutes. Pexels requires photographer credit, and the page shows it in the bottom-left corner. Don't remove it.

## Running it as a dedicated display

For a TV or wall monitor, open the page in kiosk mode so it goes full screen without a click. For example, with Chrome or Chromium:

```
chrome --kiosk file:///path/to/index.html
```

Adjust the command for your operating system. Set the display to never sleep in your OS power settings as well, since the in-page screen wake lock is not supported by every browser.

## Hosting and API key safety

The Pexels key sits in plain text inside the HTML file, and browser code can't hide it.

- For personal use on your own screen, that is fine.
- Don't publish the file with your key in it. Anyone who opens the page can read the key.
- If you want to host it publicly, use a small server-side proxy for the Pexels request, or leave `PEXELS_KEY` empty and rely on Picsum.
- If a key was ever shared by accident, generate a new one in your Pexels account.

## Browser support

Any recent Chrome, Edge, Firefox or Safari. The page uses modern CSS such as `color-mix()` and `backdrop-filter`, so very old browsers will not display it correctly. On a portrait screen the layout stacks into one column and scrolls.

## Troubleshooting

| Problem | What to try |
|---|---|
| Only a moving gradient, no photos | Open the file in a regular browser tab. Check that the internet works and no ad blocker is blocking `api.pexels.com` or `picsum.photos`. Press F12 and look in the Console for red errors. |
| Fonts look plain | Google Fonts didn't load. The page works but uses system fonts. Check your connection. |
| Prayer times differ from the mosque | Use `OFFSETS`, or check `ASR_FACTOR`. |
| Wrong world-clock time | Check that `tz` is a valid IANA name, such as `America/Halifax`. |
| Screen goes to sleep | Use kiosk mode and change the OS power settings. |

## Project files

| File | Notes |
|---|---|
| `bangladesh-clock-v6.html` | The current version. Use this one. |
| `bangladesh-clock.html`, `bangladesh-clock-v2.html` to `v5.html` | Earlier iterations, kept for reference. |
| `README.md` | This file. |

## Credits

- Photos: [Pexels](https://www.pexels.com) and photographers credited on screen, [Picsum](https://picsum.photos) (Unsplash), and Bing where available.
- Fonts: Sora, Plus Jakarta Sans and Hind Siliguri, served by Google Fonts.
- Prayer-time formulas follow the widely used solar-position method popularized by PrayTimes.org.
