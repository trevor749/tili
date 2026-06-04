# thisislongisland.com — WordPress Install (Plug & Play)

**For the web developer.** This is a complete, self-contained landing site. Two pages. No build step. No frameworks. Drop into WordPress in under 30 minutes.

---

## TL;DR

1. Get the two HTML files from the repo
2. Create two WordPress pages, paste each file into a Custom HTML block
3. Set one as the homepage
4. Submit one form on each page to activate Formsubmit
5. Done — live

If you need more detail, read on.

---

## Step 1: Get the files

Repo: **https://github.com/trevor749/tili**

Clone, fork, or download as zip. You only need two files for production:

| File | What it becomes |
|---|---|
| `tili-home.html` | The homepage at `thisislongisland.com/` |
| `updates.html` | The email-capture page at `thisislongisland.com/updates` |

Everything else in the repo (`audit-*.html`, `founding-5-*.html`, `amtb-studio.html`, `thank-you-ghl.html`, `index.html`) is unrelated preview infrastructure. Ignore it.

---

## Step 2: Install in WordPress

Three paths. Pick whichever fits the existing WordPress setup. **Path A is fastest and recommended.**

### Path A — Custom HTML Block (no theme changes)

Works on any WordPress install with the block editor (Gutenberg).

1. **WP Dashboard → Pages → Add New**
2. Title: **Home** (slug doesn't matter for the homepage)
3. Click the **+** button → search for **"Custom HTML"** → add the block
4. Open `tili-home.html` in any text editor, select all (Cmd+A / Ctrl+A), copy
5. Paste into the Custom HTML block
6. Click **Publish**
7. Repeat for the second page: title **Updates**, slug `updates`, paste `updates.html`
8. **WP Settings → Reading → "A static page"** → Front page = **Home**

**If the active theme adds a header / sidebar / footer that interferes with the layout:**
- Edit the page → right sidebar → **Page Attributes → Template** → choose **"Blank"**, **"Full Width"**, **"No Sidebar"**, or **"Canvas"** (varies by theme)
- If none of those options exist, install a free clean theme: **Astra**, **GeneratePress**, or **Kadence**. Each provides a "Blank Canvas" template that strips all theme chrome.

### Path B — Page Template (theme-integrated, cleanest)

For when this is going on a production WordPress with proper version control on the theme.

1. SSH or FTP into `wp-content/themes/[active-theme]/`
2. Create `page-home.php`. First line must be:
   ```php
   <?php /* Template Name: TILI Home */ ?>
   ```
   Paste the entire contents of `tili-home.html` below that line.
3. Create `page-updates.php` similarly with `Template Name: TILI Updates`.
4. WP Dashboard → Pages → Add New → "Home" → right sidebar → **Page Attributes → Template** = **TILI Home**. Same for Updates.

### Path C — Page Builder (Elementor / Divi / Bricks)

1. New page → set template to **Blank** or **Canvas**
2. Add an **HTML widget** (Elementor) / **Code module** (Divi) / **HTML element** (Bricks)
3. Paste the full file contents
4. Save

---

## Step 3: Activate the forms (5 minutes, one time only)

Both forms send submissions directly to **trevor@thisislongisland.com** via [Formsubmit.co](https://formsubmit.co) — free, no account, no monthly cost.

**You must do this once on each page before submissions will arrive in the inbox:**

1. After publishing, open the **live homepage** in a browser
2. Scroll to the reservation form
3. Fill out with real values (use your own email is fine), click **Reserve My Spot**
4. Check **trevor@thisislongisland.com** inbox for an email from Formsubmit titled "Confirm Email"
5. Click the **activation link** in that email
6. Repeat steps 1–5 on the `/updates` page using its email signup form

After activation: every form submission goes straight to the inbox, formatted as a clean HTML table with every field.

**Email subjects:**
- Reservation form → `New Studio Reservation: [Name] - [Organization]`
- Updates page → `New Updates Subscriber: [email]`

**Optional security upgrade after activation:** Log into Formsubmit, grab the hashed endpoint (looks like `https://formsubmit.co/abc123XYZ`), and search-and-replace `https://formsubmit.co/ajax/trevor@thisislongisland.com` in both files. This hides the email address from public page source.

The variables to find:
- `RESERVE_ENDPOINT` in `tili-home.html`
- `UPDATES_ENDPOINT` in `updates.html`

---

## Step 4: Connect QuickBooks for the $2,960 deposit (optional)

The reservation form captures leads. The actual deposit gets collected via QuickBooks. Two ways to wire it:

### Option A — QuickBooks Payment Link (instant pay at booking)

1. QuickBooks Online → **Sales → Payment Links → Create payment link**
2. Amount: **$2,960** + NY tax line (~8.625% ≈ $255 → total ~$3,215)
3. Description: "AMTB Workforce Studio Deposit"
4. **Create** → copy the URL (looks like `https://payments.intuit.com/...`)
5. In `tili-home.html`, locate the gold **Reserve My Spot** button inside the form. Either:
   - **Replace the form** with a single button linking to that URL (instant pay, no form fields), OR
   - **Keep the form for lead capture** and manually email the QB Payment Link to qualified leads (most common for B2B)

### Option B — Auto-create QB invoice from form submission

1. Sign up at zapier.com (free tier covers 100 submissions/month — plenty for a pilot)
2. **Create Zap → Trigger = "Email by Zapier"** → connect to trevor@thisislongisland.com → filter rule: subject contains "New Studio Reservation"
3. **Action 2 = QuickBooks Online → Create Invoice** with $2,960 + tax, sent to the email in the body
4. Turn Zap on. Reservations now auto-generate QB invoices.

**Cost:** $0/month either option. Only QB Payments transaction fees (~2.99% + $0.25) when someone actually pays.

---

## Step 5: Point the domain

1. Update DNS A/CNAME records for **thisislongisland.com** to point at the WordPress host
2. WP Dashboard → **Settings → General**:
   - Site Address (URL): `https://thisislongisland.com`
   - WordPress Address (URL): `https://thisislongisland.com`
3. Install an SSL cert (most hosts: free via Let's Encrypt)
4. Done

---

## Common gotchas

### Layout looks broken — header/footer/sidebar appearing
The active WP theme is wrapping the page content in its own chrome. Use a "Blank Canvas" or "Full Width" template (see Step 2 Path A). Or install Astra/GeneratePress.

### Animations/CTAs not working
The JavaScript at the bottom of each file must be intact. Make sure the Custom HTML block didn't strip `<script>` tags. Right-click the live page → View Source → search for `formsubmit.co`. If it's there, JS is loaded.

### Google Fonts not loading
The HTML loads Anton + Bebas Neue + Inter + Raleway from Google Fonts CDN. Works on any WordPress install with internet access. If the host blocks external requests, install the free **OMGF** plugin to self-host the fonts.

### Form submissions aren't arriving in the inbox
You haven't activated Formsubmit yet. Step 3 above. Check spam folder for the activation email.

### Partner event URL changed
Both files reference this URL in 2–3 spots:
```
https://sportspowerinfrastructure.com/events/athletes-make-the-best-tm-launches-pilot-student-athlete-workforce-studio-with-long-island-media-and-the-spin-foundation
```
If Sports Power Infrastructure changes the URL, do a global find-and-replace across both files before deploying.

---

## Design system reference (for future edits)

Both files share the same CSS custom properties in `:root`:

```css
:root {
  --teal: #1bb0ce;        /* Primary brand */
  --teal-bright: #22e8ff; /* Hover / accent */
  --gold: #f5b731;        /* Deposit / premium CTA */
  --gold-bright: #ffd45c; /* Gold hover */
  --ink: #050505;         /* Page background */
  --panel: #0a0a0a;       /* Card background */
  --line: #1a1a1a;        /* Borders */
  --mute: #888;           /* Secondary text */
}
```

**Fonts (Google Fonts CDN):**
- **Anton** — massive display headlines
- **Bebas Neue** — eyebrows, nav, sports-broadcast feel
- **Inter** — body text
- **Raleway** — italic accents

To change brand colors site-wide, edit the `--teal` and `--gold` values in `:root`. Everything updates automatically.

---

## Page architecture

### `tili-home.html`
- Fixed top nav: brand mark | Updates link | gold "● Reserve" CTA
- Hero — the event itself (AMTB Workforce Studio) with gold Reserve CTA
- Marquee
- **About The Pilot** — 4 symmetric beat cards (Problem · Solution · Studio · Outcome)
- Marquee (reversed)
- Partners (AMTB, Long Island Media, SPIN Foundation)
- **Reserve form** — emails to trevor@thisislongisland.com
- Final CTA
- Footer

### `updates.html`
- Same nav with "Studio Event" link back to home
- Hero with single email capture form
- Footer

---

## Live previews while you build

These GitHub Pages URLs let you preview the pages before WordPress is live:

- Main: https://trevor749.github.io/tili/tili-home.html
- Updates: https://trevor749.github.io/tili/updates.html

These are throwaway preview URLs — they retire once thisislongisland.com is live.

---

## Questions

Any blockers, hit up Trevor directly. The HTML is intentionally simple — no JS frameworks, no build tools, no node_modules. View the source if you're unsure what something does. Everything is inline and commented.
