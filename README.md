# Aram Labs Plugins

Claude Code plugin marketplace for Aram Labs clients and internal tools.

## Installation

Add this marketplace to Claude Code:

```
/plugin marketplace add Aram-Labs/aramlabs-plugins
```

Then browse available plugins in the `/plugin` Discover tab.

## Available Plugins

| Plugin | Description | Category |
|--------|-------------|----------|
| [aram-starter](./plugins/aram-starter/) | Aram Labs design system conventions, subdomain patterns, and auth-gate usage | Development |
| [surge-toolkit](./plugins/surge-toolkit/) | Deploy static websites to Surge.sh with optional password protection | Deployment |

## For Aram Labs Team

### Adding a New Plugin

1. Create a directory under `plugins/<plugin-name>/`
2. Add `.claude-plugin/plugin.json` with the plugin manifest
3. Add skills, commands, agents, or hooks as needed
4. Register the plugin in `.claude-plugin/marketplace.json`
5. Commit and push — clients will see the new plugin on next `/plugin` refresh

### Plugin Structure

```
plugins/<plugin-name>/
  .claude-plugin/
    plugin.json           # Required: plugin manifest
  skills/                 # Optional: skill files
    <skill-name>/
      SKILL.md
  commands/               # Optional: slash commands
    <command>.md
  agents/                 # Optional: custom agents
    <agent>.md
  hooks/                  # Optional: event hooks
    hook-config.json
  README.md               # Plugin documentation
```

### Plugin Manifest (plugin.json)

```json
{
  "name": "plugin-name",
  "description": "What this plugin does",
  "version": "1.0.0",
  "author": {
    "name": "Aram Labs",
    "email": "sen@aramlabs.ca"
  }
}
```

## Links

- [Aram Labs](https://aramlabs.ca)
- [GitHub Org](https://github.com/Aram-Labs)
