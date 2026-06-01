# CLAUDE.md

Guidance for Claude Code when working in this repository.

## Project

Shopify storefront theme for **saltsunsand.com.au**.

- **Base theme:** Concept by RoarTheme (`config/settings_schema.json` → `theme_info`)
- **Theme version:** 5.3.3
- This is a live commerce theme — changes here ship to a real storefront. Prefer
  small, reviewable edits and validate Liquid before pushing.

## Layout (standard Shopify theme)

| Folder       | Contents                                                              |
|--------------|----------------------------------------------------------------------|
| `assets/`    | CSS, JS, fonts, images. Large bundles: `theme.css`, `theme.js`, `vendor.js` |
| `blocks/`    | Reusable theme blocks (incl. several `ai_gen_block_*.liquid`)         |
| `config/`    | `settings_schema.json` (editable settings) + `settings_data.json` (saved values) |
| `layout/`    | `theme.liquid` (main), `password.liquid`                              |
| `locales/`   | Translations; `*.json` for storefront, `*.schema.json` for the editor |
| `sections/`  | ~101 page sections                                                    |
| `snippets/`  | ~98 reusable Liquid partials                                          |
| `templates/` | Page templates — see note below                                      |

## Templates: JSON vs Liquid

Most templates are **JSON** (e.g. `product.json`, `index.json`) — they reference
sections + blocks and store editor settings. A few are **Liquid** (e.g.
`gift_card.liquid`). There are multiple variants per page type (e.g.
`collection.bean-bag.json`, `product.cushions.json`, preset variants) — make sure
you edit the variant that's actually assigned to the page in question.

## Conventions

- **Edit sections/snippets, not the JSON templates,** for markup/logic changes.
  JSON templates mostly hold settings and section ordering.
- **Don't hand-edit `config/settings_data.json`** unless intentional — it's
  normally written by the Shopify theme editor and merge conflicts here are painful.
- **Don't edit `assets/theme.css`, `assets/theme.js`, or `assets/vendor.js` by hand**
  unless necessary — they're large, generated/minified vendor bundles.
- **User-facing strings** belong in `locales/*.json`, referenced via `t:` keys.

## Validation

Lint Liquid before pushing:

```bash
shopify theme check        # uses .theme-check.yml
```

Local preview against the store:

```bash
shopify theme dev --store saltsunsand.myshopify.com
```

## Git & branch model

- **Default branch:** `main` — the canonical state of the theme.
- **Active development branch:** `claude/awesome-pascal-DmIPy` — do work here.
- Do **not** push to branches other than the active dev branch without explicit
  permission.
- `main` and the dev branch started identical at the initial import.

### Per-session workflow (do this every session)

1. Make changes on the active dev branch (`claude/awesome-pascal-DmIPy`).
2. Validate with `shopify theme check` before committing.
3. Commit with a clear message and push the dev branch.
4. Merge to `main` when the change is approved — via PR, or a direct push to
   `main` **only when the user explicitly authorizes it that session**.

## Deploying to the live storefront

Theme code in this repo does **not** auto-deploy. Shipping to
saltsunsand.com.au is done with the Shopify CLI, which must run on a machine
authenticated to the store's Shopify account (i.e. the user's local machine —
not this repo's CI/remote session):

```bash
# One-time install
npm install -g @shopify/cli @shopify/theme

shopify theme dev  --store saltsunsand.myshopify.com   # live-reload preview
shopify theme push --store saltsunsand.myshopify.com   # deploy to a theme
shopify theme pull --store saltsunsand.myshopify.com   # pull live theme back into repo
```

When the user asks to "deploy" or "go live" from a remote/web session, remind
them this step runs locally with Shopify CLI auth.
