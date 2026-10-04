# Drupal Gallery API — Today's APOD Custom Module

This is the Drupal 11 backend module for the APOD Gallery feature. For the Next.js frontend that consumes this module, see: https://github.com/franciscojavierguardado101/Gallery-API---Today-s-APOD-Custom-Module

## What it does

Provides a Drupal 11 paragraph type (`apod`) that integrates NASA's Astronomy Picture of the Day into a headless Drupal + Next.js architecture. The module:

- Registers a `apod` paragraph type so editors can drop the APOD component onto any landing page via the Drupal CMS
- Exposes a JSON config endpoint at `/api/apod/config` that the Next.js frontend reads to get the NASA API key, gallery image count, and cache duration
- Keeps the NASA API key stored securely in Drupal config, never hardcoded or exposed to the browser
- Provides an admin settings form at `/admin/config/francisco/apod` to control all settings without touching code

## Admin settings

| Setting | Description | Default |
|---------|-------------|---------|
| NASA API Key | Get a free key at api.nasa.gov. Use `DEMO_KEY` for testing (30 req/hr limit) | `DEMO_KEY` |
| Gallery image count | Number of random APOD images shown in the frontend gallery row | `6` |
| Cache duration | How long Next.js caches responses via ISR (seconds) | `3600` |

## Installation

Requires Drupal 10 or 11 with the `paragraphs` module enabled.

```bash
# Copy module into your Drupal installation
cp -r francisco_apod /path/to/web/modules/custom/

# Enable via Drush
drush en francisco_apod -y
drush cr
```

## File structure

```
francisco_apod/
  francisco_apod.info.yml          # Module definition, dependencies
  francisco_apod.routing.yml       # Route: /api/apod/config
  francisco_apod.links.menu.yml    # Admin menu link
  francisco_apod.libraries.yml     # CSS library
  francisco_apod.module            # Hook implementations
  francisco_apod.schema.yml        # Config schema
  src/
    Controller/
      ApodController.php           # JSON endpoint for Next.js
    Form/
      ApodSettingsForm.php         # Admin UI: API key, count, cache TTL
  templates/
    paragraph--apod.html.twig      # Twig template (server-side fallback)
  css/
    apod.css                       # Component styles
  config/install/
    francisco_apod.settings.yml    # Default config
    paragraphs.paragraphs_type.apod.yml
    field.storage.paragraph.field_apod_label.yml
    field.field.paragraph.apod.field_apod_label.yml
    core.entity_form_display.paragraph.apod.default.yml
    core.entity_view_display.paragraph.apod.default.yml
```

## Stack

- Drupal 11 (headless, Pantheon)
- PHP 8.2
- Paragraphs module
- Next.js 16 frontend on Vercel (see link above)
