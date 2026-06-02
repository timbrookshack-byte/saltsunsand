# saltsunsand.com.au

Shopify storefront theme for **saltsunsand.com.au**.

- **Base theme:** Concept by RoarTheme
- **Theme version:** 5.3.3

## Structure

Standard Shopify theme layout:

| Folder       | Purpose                                                        |
|--------------|----------------------------------------------------------------|
| `assets/`    | CSS, JS, images and other static files                         |
| `blocks/`    | Reusable theme blocks                                          |
| `config/`    | Theme settings (`settings_schema.json`, `settings_data.json`)  |
| `layout/`    | Top-level layouts (`theme.liquid`, `password.liquid`)          |
| `locales/`   | Translation / localization files                               |
| `sections/`  | Page sections                                                  |
| `snippets/`  | Reusable Liquid partials                                       |
| `templates/` | Page templates (product, collection, page, etc.)              |

## Local development

This theme is intended to be developed with the [Shopify CLI](https://shopify.dev/docs/themes/tools/cli).

```bash
# Install the Shopify CLI (one-time)
npm install -g @shopify/cli @shopify/theme

# Connect to the store and start a live-reloading dev server
shopify theme dev --store saltsunsand.myshopify.com

# Push changes to a theme on the store
shopify theme push

# Pull the latest live theme down into this repo
shopify theme pull
```

> **Note:** `shopify theme push/pull` must run on a machine signed in to the
> store's Shopify account. Code in this repo does **not** auto-deploy — pushing
> to GitHub and deploying to the storefront are separate steps.

## Branches & workflow

- **`main`** — canonical state of the theme (default branch).
- **`claude/awesome-pascal-DmIPy`** — active development branch; changes are made
  here, then merged into `main` once approved.

Typical loop:

1. Edit on the dev branch.
2. `shopify theme check` to lint Liquid.
3. Commit and push.
4. Merge into `main`.
5. Deploy to the live store with `shopify theme push` (run locally).

See `CLAUDE.md` for theme structure, conventions, and gotchas.
See `VERIFYING.md` for how to push to a staging theme and verify changes.
