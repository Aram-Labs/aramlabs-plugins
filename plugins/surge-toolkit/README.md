# Surge Toolkit

Deploy static websites to Surge.sh with optional client-side password protection — no paid services needed.

## Commands

### `/surge-setup`

Checks your environment and installs the Surge CLI if needed:

1. Verifies Node.js is available
2. Installs `surge` globally if missing
3. Checks login status
4. Generates a deployment token for CI use

### `/surge-deploy`

Guided deployment with optional password protection:

1. Finds your source directory (looks for `index.html`)
2. Lets you choose a domain (`*.surge.sh` or custom)
3. Sets up SPA routing support (`200.html`)
4. Optionally adds client-side password protection
5. Deploys to Surge.sh

## Password Protection

The auth gate provides simple password protection using:

- **SHA-256 hashing** via Web Crypto API (no plaintext passwords in source)
- **localStorage persistence** so returning visitors aren't re-prompted
- **No backend required** — everything runs client-side
- **No paid services** — works with Surge.sh free tier

**Note:** This is obfuscation, not true security. The hash is visible in source code. Use it to keep casual visitors out, not to protect sensitive data.

## Custom Domains

For `*.surge.sh` subdomains, no DNS setup is needed. For custom domains, add a CNAME record:

```
CNAME  your-subdomain  na-east1.surge.sh
```

## Installation

This plugin is part of the `aramlabs-plugins` marketplace:

```
/plugin marketplace add Aram-Labs/aramlabs-plugins
```

Then install the `surge-toolkit` plugin from the Discover tab.
