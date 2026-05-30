# registry

This is the official registry for all the plugins of Advanced Discord Bot. This is the recommended way to upload community plugins to the marketplace.

**Submission Flow:**
1. Developers create plugins and publish them to npm.
2. Developers submit a PR this repo. (More about this below)
3. Maintainers review and merge the PRs.
4. Plugin appears in marketplace automatically the moment we merge the PR.

## Submitting your Plugin

### Plugin Entry Reference

Each plugin entry supports these fields:

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Internal name (npm package name) |
| `displayName` | string | Yes | Human-readable name |
| `description` | string | Yes | What the plugin does |
| `author` | string | Yes | Developer name |
| `version` | string | Yes | Current version |
| `category` | string | Yes | Category (Category must be one of these : `core features`,`moderation`,`utility`,`analytics`,`entertainment`) |
| `permissions` | array | No | Required permissions |
| `requiresRestart` | boolean | No | Needs bot restart to apply |
| `verified` | boolean | No | Official/audited plugin |
| `npmPackage` | string | Yes | Exact npm package name |
| `port` | number | No | Dashboard port if applicable |
| `configSchema` | object | No | Settings schema |

- These are the expected keys in the json you see below.

### Fork the repo and edit [plugins.json](./plugins.json) 

Add your Plugin to `plugins.json` :

```json
{
  "plugins": [
    {
      "name": "vaish-plugin-economy",
      "displayName": "Economy System",
      "description": "Complete economy system with coins, work commands, shop, and leaderboards",
      "author": "VAISH",
      "version": "1.0.0",
      "category": "features",
      "permissions": ["db.read", "db.write", "commands.register"],
      "requiresRestart": false,
      "verified": true,
      "npmPackage": "vaish-plugin-economy"
    },
    ...
    ...
    #your plugin here
  ]
}
```

### Open a PR and wait for approval.

## Plugin Requirements

- Must have a valid `plugin.json` manifest
- Must be published to npm
- Must not break the bot
- Must follow VAISH conventions

## Review Process

 We Check for:
- Valid plugin.json
- Working npm package
- No security issues
