# Alom DigiTech — Landing Page

## Overview
This repo contains a static marketing landing page for **Alom DigiTech** ("Build. Automate. Scale."). The page lives in `landing/index.html` (self-contained HTML/CSS/JS, no build step).

## Running locally
```bash
docker compose -f docker-compose.base44.yml up -d
```
Serves the landing page on **port 3000** via nginx.

## Editing
- All content and styling is in `landing/index.html` — no framework, no dependencies, no build.
- Fonts load from Google Fonts CDN; everything else is inline.
- To change copy, edit the HTML directly. To change colors, edit the `:root` CSS variables.
