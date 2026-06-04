# thisislongisland.com — Handoff Notes

For the web guy taking this into WordPress.

---

## What's in the package

Two pages, ready to drop in:

| File | Becomes | Purpose |
|---|---|---|
| `tili-home.html` | `thisislongisland.com/` (home) | Main landing. Hero is the AMTB Workforce Studio event. Has the reservation form. |
| `updates.html` | `thisislongisland.com/updates` | Email-capture "more coming soon" landing page, linked from the home nav. |

Both files are **single self-contained HTML files**. Inline CSS. Inline JS. Only external dependency is Google Fonts (CDN). No build step. No node_modules.

---

## How to deploy in WordPress (pick one)

### Option A — Fastest (Custom HTML block)
1. Create a new page in WordPress
2. Add a **Custom HTML** block
3. Paste the entire contents of `tili-home.html`
4. Publish, set as homepage. Repeat for `updates.html` as `/updates`.

### Option B — Cleaner (page template)
1. Convert each HTML file into a PHP page template under your active theme: `page-home.php`, `page-updates.php`
2. WordPress dashboard → Pages → set the template on each page

### Option C — Elementor / Divi / Bricks
The CSS uses variables (`--teal`, `--gold`, etc.). Convert sections one-by-one — each `<section>` becomes a builder section. Inline styles drop into the builder's custom CSS field.

---

## Three things to wire up before going live

### 1. The Reservation Form → QuickBooks
Inside `tili-home.html`, search for this line near the bottom:
```js
var RESERVE_WEBHOOK = 'YOUR_WEBHOOK_URL';
```

**Cheapest path (recommended):** Connect this to a free Zapier Catch Hook that creates a QuickBooks invoice automatically.

1. Zapier → Create Zap → Trigger = **Webhooks → Catch Hook** → copy the hook URL
2. Paste that URL in place of `YOUR_WEBHOOK_URL`
3. Zapier action = **QuickBooks Online → Create Invoice** for $2,960 + NY tax
4. Turn Zap on

Free tier on Zapier handles 100 submissions per month.

**Alternative (cheapest with zero middleware):** Skip the form for instant payment. Replace the gold "Reserve My Spot" CTA with a direct link to a QuickBooks Payment Link:
- In QuickBooks Online → Sales → Payment Links → create one for $2,960 + tax
- Change the form CTA's `href` to that QB payment URL

### 2. The Updates Page Email Capture
Inside `updates.html`, search for:
```js
var UPDATES_WEBHOOK = 'YOUR_WEBHOOK_URL';
```

Cheapest options:
- **Formspree free tier** — 50 submissions/month, no credit card needed. Sign up → create form → paste the form endpoint URL.
- **Zapier free tier** — same hook pattern as above; second Zap, action = "add subscriber to Mailchimp" or "send email to me."

### 3. Partner Event URL
Both files reference this partner page in 2–3 spots:
```
https://sportspowerinfrastructure.com/events/athletes-make-the-best-tm-launches-pilot-student-athlete-workforce-studio-with-long-island-media-and-the-spin-foundation
```
If the partner changes the URL, do a find-and-replace across both files.

---

## Page architecture

### `tili-home.html`
- **Top nav** — brand mark left, "Updates" link + gold "Reserve" CTA right
- **Hero** — the event itself. Gold "Reserve My Spot" → scrolls to form
- **Marquee** — scrolling band
- **About The Pilot** — 4 cards (Problem · Solution · Studio · Outcome)
- **Marquee** (reverse direction)
- **Partners** — AMTB, Long Island Media, SPIN Foundation + hosted-by SPI tag
- **Reserve form** — captures lead, ready to wire to webhook
- **Final CTA** — one more "Reserve My Spot" button
- **Footer** — wordmark, email, links

### `updates.html`
- **Top nav** — brand mark left, "Studio Event" link + gold "Reserve" CTA right
- **Hero with email capture** — single screen, one input + button
- **Footer** — same as above

---

## Domain swap checklist

Search-and-replace these strings when going live on the real domain:

| Find | Replace with |
|---|---|
| `tili-home.html` | `/` or `/home/` (whatever the WP home slug is) |
| `updates.html` | `/updates/` |
| `mailto:hello@thisislongisland.com` | (already correct) |

---

## Design system reference

For anyone updating colors or fonts later, the CSS variables in both files:

```css
:root {
  --teal: #1bb0ce;        /* primary brand */
  --teal-bright: #22e8ff; /* accent / hover */
  --gold: #f5b731;        /* deposit, premium, CTA accents */
  --gold-bright: #ffd45c; /* gold hover */
  --ink: #050505;         /* background */
  --panel: #0a0a0a;       /* card background */
  --line: #1a1a1a;        /* borders */
  --mute: #888;           /* secondary text */
}
```

Fonts (loaded from Google Fonts):
- **Anton** — massive display headlines
- **Bebas Neue** — eyebrows, navs, sports-broadcast feel
- **Inter** — body text
- **Raleway** — italic accent text

---

## Live previews while we develop

These are the temporary GitHub Pages URLs for preview/sharing during build (not the production URLs):

- Main: https://trevor749.github.io/tili/tili-home.html
- Updates: https://trevor749.github.io/tili/updates.html
- Partner event (the SPI page we link to): https://sportspowerinfrastructure.com/events/athletes-make-the-best-tm-launches-pilot-student-athlete-workforce-studio-with-long-island-media-and-the-spin-foundation

These die the moment the real WordPress site goes live and serves the same HTML.
