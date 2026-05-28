# Week Dinners — TRMNL X Plugin

Display the school-week meal schedule on a **TRMNL X** (1872×1404, 4-bit grayscale). The plugin reads meal data from `weekdays.json` hosted on GitHub Pages — no server required.

## How it works

```
weekdays.json (GitHub Pages)
        ↓  polling
TRMNL Private Plugin (Liquid markup)
        ↓  render
TRMNL X e-ink display
```

1. You edit `weekdays.json` in this repo and push to `main`.
2. GitHub Pages publishes the JSON at a public URL.
3. Your TRMNL device polls that URL on a schedule you choose.
4. TRMNL merges the JSON into the Liquid template and renders it on screen.

---

## Prerequisites

- A [TRMNL](https://trmnl.com) account with a **TRMNL X** device set up
- This repo deployed to GitHub Pages (already configured via `.github/workflows/deploy.yml`)
- The JSON URL must be publicly reachable:
  ```
  https://bebop-minsann.github.io/week-dinners/weekdays.json
  ```

---

## Files

| File | Purpose |
|------|---------|
| [`full.liquid`](full.liquid) | Markup template — paste into the Private Plugin **Full** tab |
| [`preview.html`](preview.html) | Local browser preview with sample data |

---

## Setup (one-time)

### 1. Create the Private Plugin

1. Log in at [trmnl.com](https://trmnl.com).
2. Go to **Plugins → Create Private Plugin**.
3. Give it a name, e.g. **Week Dinners**.

### 2. Configure polling

| Setting | Value |
|---------|-------|
| **Strategy** | Polling |
| **Polling URL** | `https://bebop-minsann.github.io/week-dinners/weekdays.json` |
| **Refresh interval** | 6–12 hours (meals change weekly; longer saves battery) |

Save the plugin settings.

### 3. Add the markup

1. From the plugin settings page, click **Edit Markup**.
2. Open the **Full** tab.
3. Copy the entire contents of [`full.liquid`](full.liquid) and paste them in.
4. Click **Force Refresh** to fetch the JSON and load merge variables (`weekdays`, etc.).
5. In the preview panel, select **TRMNL X** in the device picker (top-right).
6. Confirm all five weekdays render with snack, lunch, and dinner.
7. Save the markup.

### 4. Add to your device playlist

1. Go to your TRMNL device settings or playlist.
2. Add the **Week Dinners** plugin as a full-screen item.
3. Wait for the next sync, or trigger a refresh from the TRMNL app.

The schedule should appear on your TRMNL X in landscape with five columns (Mon–Fri).

---

## Updating meals

Edit [`weekdays.json`](../weekdays.json) in the repo root:

```json
{
  "weekdays": [
    {
      "day": "Monday",
      "snack": "Peanut butter hardbread, fruit",
      "lunch": "Avocado sandwich with tomato and cucumber",
      "dinner": "Meatballs and potatoes"
    }
  ]
}
```

Each entry needs four fields: `day`, `snack`, `lunch`, `dinner`. Include all five weekdays (Monday–Friday).

Then:

1. Commit and push to `main`.
2. Wait for GitHub Pages to redeploy (usually 1–2 minutes).
3. On TRMNL, click **Force Refresh** on the plugin, or wait for the next poll.

---

## Local preview

Before pasting markup into TRMNL, you can preview locally:

```bash
open trmnl/preview.html
```

Or serve the repo and open `/trmnl/preview.html` in a browser.

`preview.html` loads the TRMNL Framework CSS/JS from CDN and uses hardcoded sample data. For the most accurate TRMNL X preview, paste the markup into the [Framework docs](https://trmnl.com/framework) editor and select **TRMNL X** in the device picker.

---

## Layout

| Orientation | Layout |
|-------------|--------|
| **Landscape** (default) | 5-column grid — one column per weekday |
| **Portrait** | Days stack vertically (`portrait:grid--cols-1`) |

Each day card shows:
- Day name (heading)
- Snack, Lunch, Dinner (label + description)

Typography uses TRMNL Framework defaults (Inter on TRMNL X). Colors are grayscale only — card backgrounds use `bg--gray-75` (light gray). In TRMNL's grayscale scale, **low numbers are dark** (`gray-10` ≈ black) and **high numbers are light** (`gray-75` ≈ off-white).

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Blank screen or missing days | Click **Force Refresh** in the plugin editor. Confirm the polling URL returns valid JSON. |
| Variables like `{{ day.snack }}` show literally | Force Refresh hasn't loaded data yet, or the JSON root key isn't `weekdays`. |
| Black cards, unreadable text | Re-paste markup from `full.liquid` — cards must use `bg--gray-75`, not `bg--gray-10`. |
| Changes not appearing on device | Push to `main`, wait for GitHub Pages deploy, then Force Refresh or wait for the poll interval. |
| Text cut off on long meals | Descriptions use `data-clamp="4"`. Shorten text in JSON or increase the clamp value in `full.liquid`. |

---

## Further reading

- [TRMNL Private Plugins](https://help.trmnl.com/en/articles/9510536-private-plugins)
- [TRMNL X Framework guide](https://trmnl.app/framework/docs/3.1/trmnl_x_guide)
- [Grayscale / bit-depth reference](https://help.trmnl.com/en/articles/12386214-grayscale-1-bit-2-bit-4-bit-in-framework)
