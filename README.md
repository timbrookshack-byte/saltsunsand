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
