---
name: aram-starter
description: Aram Labs design system and conventions. Use when building sites, documents, or tools for Aram Labs or its clients (WealthBlume, L Squared, Arbour Oak, Grizzly, TBDC).
---

# Aram Labs Conventions

## Design System (aramlabs.css)

All Aram Labs document sites use a shared stylesheet loaded from CDN:

```html
<link rel="stylesheet" href="https://aramlabs.ca/aramlabs.css">
```

### Key Principles
- **Light theme by default** — dark theme via `data-theme="dark"` on `<html>`
- **CSS custom properties** for all colors, spacing, and typography
- **Client accent overrides** via 3-5 lines of inline `<style>` (e.g., `--al-accent: #C41230` for L Squared)
- **5 templates available:** document, dashboard, presentation, calculator, review

### Common Components
- `.ag-card` — Card container with optional color-coded badges
- `.ag-section` — Content section with heading and body
- `.ag-hero` — Hero banner with gradient background
- `.ag-table` — Responsive data table
- `.ag-badge` — Status badges (amber, blue, red, green variants)

## Subdomain System

All client-facing sites deploy to `*.aramlabs.ca` subdomains via Surge.sh.

### Pattern
```
<client>-<purpose>.aramlabs.ca
```

Examples:
- `lsquared-kiosk-estimate.aramlabs.ca`
- `wealthblume-proposal.aramlabs.ca`
- `warehouse-docs.aramlabs.ca`
- `internal-contract-review.aramlabs.ca`

### DNS
Wildcard CNAME `*` points to `na-east1.surge.sh` in Cloudflare. Any subdomain works immediately after Surge deployment.

### Auth Gate

Client-facing sites include an auth gate script in `<head>`:

```html
<script data-client="<slug>" src="https://aramlabs.ca/auth-gate.js"></script>
```

Valid client slugs: `wealthblume`, `lsquared`, `arbouroak`, `grizzly`, `tbdc`, `internal`, `forge`

The auth gate prompts for a password before showing page content. Each client has a unique password.

## Deployment

All Surge deployments should follow these steps:
1. Build the static site
2. Add auth-gate script tag to HTML `<head>` if client-facing
3. Deploy via `npx surge <directory> <subdomain>.aramlabs.ca`
4. Update the subdomain registry

## File Organization

Aram Labs projects live in top-level directories:
```
Claude-Code/
  Aram Labs/          # Business website + design system
  Aram Labs Plugins/  # This marketplace
  WealthBlume/        # Client project
  L Squared/          # Client project
  TBDC Knowledge Hub/ # Client project
  Warehouse/          # Internal system
```

## GitHub

- **Org:** Aram-Labs
- **Key repos:** aramlabs.ca, aramlabs-plugins, wealthblume, wealthblume-backend, menukiosk
