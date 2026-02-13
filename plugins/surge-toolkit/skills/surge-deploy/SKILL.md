---
name: surge-deploy
description: Deploy a static website to Surge.sh with optional client-side password protection. Handles domain setup, SPA routing, auth gate injection, and deployment.
---

# Surge Deploy

Deploy static websites to Surge.sh with optional password protection — no paid services needed.

## Deployment Workflow

Follow these steps in order:

### 1. Find the Source Directory

Look for a directory containing `index.html`. Check common locations:
- Current working directory
- `dist/`, `build/`, `out/`, `public/`

If no `index.html` is found, ask the user which directory to deploy.

### 2. Choose a Domain

Ask the user what domain they want:

- **Free subdomain:** `<name>.surge.sh` (works immediately, no DNS setup)
- **Custom domain:** Any domain they own (requires DNS configuration)

### 3. Create CNAME File

Write the chosen domain into a `CNAME` file in the source directory:

```
echo "my-site.surge.sh" > <source-dir>/CNAME
```

This tells Surge which domain to deploy to on subsequent deploys.

### 4. Create 200.html for SPA Routing

If the site is a single-page app (SPA), copy `index.html` to `200.html` in the source directory:

```bash
cp <source-dir>/index.html <source-dir>/200.html
```

Surge serves `200.html` as a fallback for all routes, enabling client-side routing (React Router, Vue Router, etc.). If in doubt, create it — it's harmless for non-SPA sites.

### 5. Ask About Password Protection

Ask the user: "Do you want to add password protection to this site?"

- **If no:** Skip to step 7.
- **If yes:** Continue to step 6.

### 6. Add Password Protection

#### 6a. Get the password

Ask the user for their desired password.

#### 6b. Generate the SHA-256 hash

```bash
echo -n "thepassword" | shasum -a 256 | cut -d' ' -f1
```

#### 6c. Generate the auth gate script

Read the template from the `references/auth-gate-template.js` file in this skill's directory. Replace `__HASH_PLACEHOLDER__` with the actual hash from step 6b. Write the result to `<source-dir>/auth-gate.js`.

#### 6d. Inject the script tag

Add this as the **first** `<script>` tag in the `<head>` of `index.html` (and `200.html` if it exists):

```html
<script src="auth-gate.js"></script>
```

Place it before any other scripts to prevent content flash.

### 7. Deploy

```bash
npx surge <source-dir> <domain>
```

If the user doesn't have surge installed, it will be installed automatically via npx.

### 8. Report

Display the result:

```
Deployed successfully!
  URL: https://<domain>
  Password protected: Yes/No
```

If password-protected, remind the user what password they set.

## Custom Domain DNS Setup

### For `*.surge.sh` subdomains
No DNS setup needed — works immediately after deployment.

### For custom domains
Add a CNAME record pointing to `na-east1.surge.sh`:

```
CNAME  @  na-east1.surge.sh
```

Or for a subdomain:

```
CNAME  app  na-east1.surge.sh
```

### For wildcard subdomains
A single wildcard CNAME covers all subdomains:

```
CNAME  *  na-east1.surge.sh
```

**Note:** DNS propagation can take up to 48 hours, though it's usually much faster.

## Useful Commands

| Command | Purpose |
|---------|---------|
| `surge list` | List all deployed sites |
| `surge teardown <domain>` | Remove a deployed site |
| `surge whoami` | Check login status |
| `surge token` | Generate a CI/CD token |

## How Password Protection Works

The auth gate uses **client-side password protection** with no backend or paid services:

1. Page content is hidden immediately on load via CSS (`visibility: hidden`)
2. A fullscreen overlay with a password input is shown
3. The entered password is hashed using SHA-256 via the Web Crypto API
4. The hash is compared against the expected hash embedded in the script
5. On match: hash is stored in `localStorage`, overlay is removed, content is shown
6. On return visits: `localStorage` is checked first — if the hash matches, no prompt is shown

**Security note:** This is *obfuscation*, not true security. The hash is visible in the source code. It's suitable for keeping casual visitors out, not for protecting sensitive data. For real security, use server-side authentication.

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Stale content after deploy | Redeploy with `npx surge <dir> <domain>` to force update |
| Custom 404 page | Create `404.html` in the source directory |
| SPA routes return 404 | Ensure `200.html` exists (copy of `index.html`) |
| Password overlay flashes content | Ensure auth-gate script is the **first** `<script>` in `<head>` |
| `surge login` fails in Claude | Run `surge login` in a separate terminal — it needs interactive stdin |
| DNS not resolving | Wait for propagation (up to 48h) or verify CNAME record points to `na-east1.surge.sh` |
