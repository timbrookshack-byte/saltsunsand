# Deploying to staging & verifying changes

How to push this theme to a **staging (unpublished) theme** on the Salt Sun Sand
Shopify store and verify the changes landed.

> **Runs locally only.** Shopify CLI must be authenticated to the store, which
> works on your local machine (browser login) — **not** from the Claude Code web
> session / this repo's CI. Code in this repo does not auto-deploy.

## 1. Get the branch locally

```bash
# first time
git clone https://github.com/timbrookshack-byte/saltsunsand.git
cd saltsunsand
git checkout claude/awesome-pascal-DmIPy

# already cloned
cd saltsunsand
git fetch origin claude/awesome-pascal-DmIPy
git checkout claude/awesome-pascal-DmIPy
git pull origin claude/awesome-pascal-DmIPy
```

## 2. Install the Shopify CLI (one-time)

```bash
npm install -g @shopify/cli @shopify/theme
```

## 3. Push to a new staging (unpublished) theme

```bash
shopify theme push --store saltsunsand.myshopify.com --unpublished --theme "Staging – awesome-pascal"
```

- First run opens the browser to log into the store (needs theme access).
- `--unpublished` creates a **new draft theme** under **Online Store → Themes**;
  the live storefront is untouched.
- It uploads the whole theme, including `config/settings_data.json` from this
  repo (the export snapshot) plus the code edits.
- **Never** use `--live` until staging has been reviewed.

`shopify theme dev --store saltsunsand.myshopify.com` is the alternative for a
*temporary* live-reload theme while editing; use `--unpublished` for a staging
theme that persists for review.

## 4. Preview

**Online Store → Themes → "Staging – awesome-pascal" → ⋯ → Preview.**
The preview URL carries `?preview_theme_id=…` — keep that param on every page you
test so you're checking the staging theme, not live. Add `&pb=0` to hide the
preview bar (e.g. for Lighthouse).

---

# Verification checklist

## SEO — heading fixes
DevTools (F12) → Console, on each page type:
```js
[...document.querySelectorAll('h1')].map(h => h.textContent.trim())
```
- **Product:** exactly one H1 = product title.
- **Home:** exactly one H1 = "Premium Outdoor Décor by Salt Sun Sand".
- **Collection:** exactly one H1 = the collection name (no literal
  `{{ collection.title }}` text on the summer-collections template).

Any result other than length `1` means a heading regression.

## Schema — JSON-LD
External validators can't fetch an unpublished theme's preview URL, so validate
from rendered HTML:

1. View Page Source (`Ctrl/Cmd+U`), `Ctrl+F` for `application/ld+json`.
2. Expect:
   - **Product:** `"@type":"Product"` with `aggregateRating` (when Ryviu is
     synced), `brand`, `offers`, `priceValidUntil` + a `BreadcrumbList`.
   - **Collection:** `CollectionPage` with `mainEntity`/`ItemList` +
     `BreadcrumbList`.
   - **Any page `<head>`:** `Organization` (with `sameAs`) and `WebSite` with
     `SearchAction`.
3. Copy each block → paste into <https://validator.schema.org/> (Code tab) or
   Google Rich Results Test → Code. Expect 0 errors.

List the types present:
```js
[...document.querySelectorAll('script[type="application/ld+json"]')]
  .map(s => JSON.parse(s.textContent)['@type'])
```

> The Product `aggregateRating` only appears when reviews are populated in the
> native `product.metafields.reviews.*` metafield. Ryviu must be configured to
> sync there, otherwise the schema is still valid but shows no stars.

## Speed — preloads & lazy-loading
- **LCP preload (collection/product):** View Source, `Ctrl+F` for
  `rel="preload" as="image"` — expect a `fetchpriority="high"` image preload in
  the `<head>`.
- **Home eager-load fix:** Console on the home page:
  ```js
  [...document.querySelectorAll('.featured-collections img, .product-grid img')]
    .map(i => i.getAttribute('loading'))
  ```
  Only the first row should be `eager`/null; the rest `lazy`.
- **Overall:** run Lighthouse (DevTools → Lighthouse → Mobile) on staging
  home/collection/product and compare LCP against the live theme.

## Copy fix (visual)
Home page → "Premium Outdoor Décor" section: the wall-art and bean-bag
paragraphs should read as complete sentences (no dangling "…each piece is:").

---

## Confirm the push / diff staging against the branch
```bash
shopify theme list --store saltsunsand.myshopify.com           # see the theme + timestamp
shopify theme pull --store saltsunsand.myshopify.com --theme <id> --path /tmp/sss-staging
# then diff the pulled copy against your working tree
```
