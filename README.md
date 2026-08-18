# untolddevotion.com

Static site for Untold Devotion Interactive. No build step, no framework, no server. Plain HTML + one CSS file — drop it on Cloudflare Pages and it runs for £0/month.

---

## What's here

```
/                     index.html          Studio front door
/games/               index.html          Games hub — catalogue of all titles
/apps/                index.html          Apps hub — UNLINKED + noindex until ready
/about/               index.html          Full About Us story
/traptap/             index.html          Trap Tap game page
/privacy/             index.html          Privacy policy (required by Google Play)
/404.html                                 Custom not-found page
/robots.txt  /sitemap.xml                 Search engine basics
/favicon.ico
/assets/site.css                          All styles (single file, brand palette as CSS vars)
/assets/brand/                            Studio marks from Brand Kit v1.1
/assets/games/traptap/                    Trap Tap icon + screenshots  ← you add these
```

---

## Before you publish — 4 things to add

### 1. Trap Tap app icon — REBUILT, VERIFY IT
The icon you sent came through as an image rather than a file, so it has been **redrawn as a vector** at `/assets/games/traptap/icon.svg` (with a 512px PNG alongside it). It is a very close match but not pixel-identical to your original.

Replace it with your real artwork: save your master as `/assets/games/traptap/icon-512.png`, then in `index.html` and `traptap/index.html` change the two `icon.svg` references to `icon-512.png`. Both pages fall back to the studio heart mark if the file is missing, so nothing ever breaks.

### 2. Screenshots
Save four phone screenshots as:
```
/assets/games/traptap/shot-1.png
/assets/games/traptap/shot-2.png
/assets/games/traptap/shot-3.png
/assets/games/traptap/shot-4.png
```
Then in `/traptap/index.html`, find the `<div class="shots">` block and replace each placeholder:
```html
<div class="shot">Drop screenshot 1 at ...</div>
```
with:
```html
<div class="shot"><img src="/assets/games/traptap/shot-1.png" alt="Trap Tap gameplay: a level with two identical buttons"></div>
```
Write real alt text describing what the screenshot shows — it helps both accessibility and search.

### 3. Social share images
When someone posts your link to WhatsApp, Discord or X, this is the picture that appears. Make two 1200×630 PNGs:
```
/assets/brand/og-image.png              Studio-branded, for the homepage
/assets/games/traptap/og-image.png      Trap Tap branded, for the game page
```
Your brand kit's `GooglePlay_Feature_Graphic_1024x500.jpg` is a good starting composition. Skip this and links will show no preview image — not fatal, but it looks unfinished.

### 4. Email address
The site uses `hello@untolddevotion.com` in three places. Set it up free with Cloudflare Email Routing (step 4 below), or search-and-replace it for an address you already have.

---

## Deploying to Cloudflare Pages

### 1. Register the domain
Cloudflare Dashboard → **Domain Registration** → **Register Domain** → search `untolddevotion.com`. Roughly $10–11/year at cost, includes WHOIS privacy and SSL. This automatically puts the domain on Cloudflare's DNS, which saves you a step later.

### 2. Put this folder in a Git repo
```bash
cd untolddevotion
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/untolddevotion.git
git push -u origin main
```
Make the repo private if you prefer — Pages works with both.

*(No Git? Cloudflare Pages also accepts a drag-and-drop zip upload. It works, but you lose auto-deploy-on-push, so you'll re-upload manually for every change.)*

### 3. Connect Pages
Cloudflare Dashboard → **Workers & Pages** → **Create** → **Pages** → **Connect to Git** → pick the repo.

Build settings — this is a static site, so leave them empty:

| Setting | Value |
|---|---|
| Framework preset | None |
| Build command | *(leave blank)* |
| Build output directory | `/` |

Deploy. You'll get a live `untolddevotion.pages.dev` URL in under a minute.

Then: **Custom domains** → **Set up a domain** → `untolddevotion.com`. Add `www.untolddevotion.com` too; Cloudflare will offer to redirect one to the other. SSL provisions automatically within a few minutes.

### 4. Email forwarding (free)
Dashboard → your domain → **Email** → **Email Routing**. Create `hello@untolddevotion.com` forwarding to your personal inbox. Costs nothing, and looks far better on a store listing than a Hotmail address.

### 5. Analytics (free, no cookie banner)
Dashboard → **Analytics & Logs** → **Web Analytics** → add your site. It gives you a small snippet to paste before `</body>` on each page. It's privacy-preserving and cookie-free, which means you don't need a consent banner — and it keeps the "no tracking" claim on your privacy page honest. Do **not** add Google Analytics unless you're prepared to add a cookie banner and rewrite that page.

---

## Site structure

Three levels, so the site can grow without a rebuild:

- **`/` — the front door.** Who you are, the Games/Apps split, your latest release, the studio values, a short About. Everything here routes deeper.
- **`/games/` and `/apps/` — the hubs.** Each owns its own hero and full catalogue. These are the pages to link from store listings and social bios.
- **`/traptap/` — the title pages.** One per game or app, with the full pitch, screenshots and store buttons.

`/apps/` is currently **unlinked and noindexed** by your choice — see "Launching the Apps page" below.

---

## Adding your second game

The site is built so this takes about ten minutes:

1. Copy the `/traptap/` folder to `/yourgame/`.
2. Edit the copy: title, meta description, canonical URL, hero copy, feature cards, the Play Store link (`?id=` package name), and the JSON-LD block at the top.
3. Add assets under `/assets/games/yourgame/`.
4. In `/games/index.html`, duplicate the `<article class="game-card">` block and point it at the new page. There's a comment marking the spot.
5. Optionally swap the homepage's "Latest release" card to the newer title.
6. Add the new URL to `sitemap.xml`.

If the new title is a paid app rather than free, remember to adjust the "Free" chip on the homepage game card and the `offers` block in that page's JSON-LD.

Nothing is hardcoded to a single game — the CSS grid handles multiple game cards without changes.

---

## Launching the Apps page

`/apps/` is built and styled but deliberately hidden — it isn't in the nav, isn't in the footer, isn't in `sitemap.xml`, and carries a `noindex, nofollow` tag so search engines skip it. You can visit it directly at `untolddevotion.com/apps/` to preview it any time.

When you have an app to publish, four small edits switch it on:

1. **Delete the `noindex` line** in `/apps/index.html` (it's marked with a comment near the top).
2. **Add it to the nav** on every page — find `<a href="/games/">Games</a>` and add `<a href="/apps/">Apps</a>` after it. Same for the footer link lists.
3. **Add the URL to `sitemap.xml`** alongside `/games/`.
4. **Swap the placeholder section** for a real entry — `/apps/index.html` has the full markup pattern in a comment block, ready to uncomment and fill in.

On the homepage, the Apps route card currently reads "In development" as plain text. Turn it into a link by changing that `<div class="card">` to `<a class="card route-card" href="/apps/">` and updating the label.

---

## Adding direct downloads later

If you ever ship a PC or web build, or want to distribute an APK outside Play:

1. Create a Cloudflare **R2** bucket (10 GB free, and crucially **zero egress fees** — a download spike can't generate a surprise bill the way S3 would).
2. Upload the build and enable public access on the bucket.
3. Add a `.btn btn-primary` link pointing at the R2 public URL alongside the store badge.

Keep game builds out of this Git repo — Pages caps individual files at ~25 MB, and a repo full of binaries becomes painful to clone.

---

## Brand notes

Colours are CSS variables at the top of `site.css`, taken from `BRAND_PALETTE.json`. The locked rules from the kit are respected here:

- Purple + gold only. No green as a third primary.
- Bright gold (`--warm-gold`) is never used for small text on cream backgrounds — only on the midnight background, where contrast is strong.
- The heart mark is used at 30px+ and only ever the simplified dark-surface version, never the detailed wreath (which requires 128px minimum).
- EB Garamond for display, Inter for UI — both loaded from Google Fonts under the OFL.
- Clear space around the mark is preserved by the flex gap on `.brand`.

If you'd rather self-host the fonts than load them from Google (marginally faster, and removes the Google Fonts mention from your privacy page), download the OFL files and swap the `<link>` for a local `@font-face` block.

---

## Google Play listing housekeeping

While you're in Play Console, point the listing at the new site:

- **Store listing → App website:** `https://untolddevotion.com/traptap/`
- **Store listing → Privacy Policy URL:** `https://untolddevotion.com/privacy/`

That privacy URL is mandatory for every app on Play, and the page must remain publicly reachable without a login for as long as the app is listed.
