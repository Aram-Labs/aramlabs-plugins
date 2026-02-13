---
allowed-tools:
  - Bash(which node)
  - Bash(node --version)
  - Bash(which surge)
  - Bash(npm install -g surge)
  - Bash(surge whoami)
  - Bash(surge token)
---

# Surge Setup

Check and configure Surge.sh CLI for static site deployment.

## Steps

### 1. Check Node.js

Run `which node` and `node --version` to verify Node.js is installed. If not found, tell the user to install Node.js first (https://nodejs.org).

### 2. Check Surge CLI

Run `which surge` to check if the Surge CLI is installed globally.

- **If installed:** Skip to step 3.
- **If not installed:** Run `npm install -g surge` to install it.

### 3. Check Login Status

Run `surge whoami` to check if the user is logged in.

- **If logged in:** Display the email and confirm setup is complete.
- **If not logged in:** Tell the user:

> Surge requires interactive login. Please run this in a **separate terminal**:
>
> ```
> surge login
> ```
>
> Enter your email and password when prompted. If you don't have an account, it will create one automatically. Then come back here and run `/surge-setup` again to verify.

**Important:** Do NOT attempt to run `surge login` via the Bash tool — it requires interactive stdin (email + password prompts) which isn't supported.

### 4. Generate Token (Optional)

If the user is logged in, run `surge token` and display the result. Explain this token can be used for CI/CD deployments by setting the `SURGE_TOKEN` environment variable.

### Summary Output

Display a summary:

```
Surge Setup Complete
  Node.js: v{version}
  Surge CLI: installed
  Logged in as: {email}
  Token: {token} (for CI use)
```
