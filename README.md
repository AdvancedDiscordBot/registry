<div align="center">

# 🧩 ADB Plugin Registry

**The official plugin marketplace index for [Advanced Discord Bot](https://github.com/AdvancedDiscordBot/Advanced-Discord-Bot).**

<br/>

![Plugins](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fraw.githubusercontent.com%2FAdvancedDiscordBot%2Fregistry%2Fmain%2Fplugins.json&query=%24.plugins.length&label=plugins&color=6A5ACD&style=for-the-badge)
![Verified](https://img.shields.io/badge/all%20listed-verified-4CAF50?style=for-the-badge)
![License](https://img.shields.io/badge/License-AGPL--3.0-blue?style=for-the-badge)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-FF9800?style=for-the-badge)](#-submitting-your-plugin)

<br/>

**For developers** — publish a plugin to npm, open a one-line PR here, and it appears in every ADB dashboard the moment it's merged.
**For server owners** — this is the catalogue your bot browses when you install plugins.

</div>

---

## 📖 Table of Contents

- [What is this?](#-what-is-this)
- [Why ADB?](#-why-adb)
- [Submitting your Plugin](#-submitting-your-plugin)
  - [Plugin Entry Reference](#plugin-entry-reference)
  - [Add your entry](#add-your-entry)
  - [Open a PR](#open-a-pr)
- [Plugin Requirements](#-plugin-requirements)
- [Review Process](#-review-process)
- [License](#-license)

---

## 🔍 What is this?

This repo holds a single file — [`plugins.json`](./plugins.json) — the **authoritative list of community plugins** available to Advanced Discord Bot. Every ADB instance reads this file to populate its in-dashboard marketplace. If your plugin is in here, users can find and install it.

**Submission flow:**

1. You build a plugin and publish it to **npm** (start from [`adb-plugin-template`](https://github.com/AdvancedDiscordBot/adb-plugin-template)).
2. You open a PR adding one entry to [`plugins.json`](./plugins.json).
3. A maintainer reviews and merges.
4. It appears in every marketplace automatically — no bot redeploy needed.

> This is the **recommended** way to distribute a community plugin. You can always `npm install adb-plugin-yourname` into a bot directly, but listing here is how users *discover* it.

---

## 💡 Why ADB?

ADB is a **self-hosted Discord bot platform** — a framework, not a fixed-feature bot.

**If you build bots:** stop rewriting the same command loader, config store, database wiring, and dashboard for every project. ADB Core handles hosting, per-guild config, storage, slash-command deployment, hot-reload, and the dashboard UI. You write a `load(ctx)` function and ship a feature — the rest is already there. A working plugin is a few files (`index.js`, `plugin.json`); the [template](https://github.com/AdvancedDiscordBot/adb-plugin-template) ships a local test harness so you can iterate with no bot and no database running.

**If you run a server:** ADB becomes exactly the bot you need. Browse the marketplace, install only the plugins you want, and configure each one from the dashboard. It's built to be safe with third-party code:

- **Sandboxed by default** — every isolated plugin runs in a worker thread. It *cannot* touch your bot token, the filesystem, the raw Discord client, or the database directly.
- **Capability-gated** — a plugin can only do what its manifest declares. Read the `permissions` / `capabilities` field before you install and you know exactly what it's allowed to do.
- **Transparent** — every listed package is public on npm and open source. You can read the code of any plugin before installing it.
- **Granular control** — enable, disable, or reconfigure any plugin per server, anytime, from the dashboard.

Install plugins from people you've never met and still know precisely what they can and can't do.

---

## 📦 Submitting your Plugin

### Plugin Entry Reference

Each entry in `plugins.json` supports these fields. This mirrors the marketplace-facing subset of your plugin's [`plugin.json` manifest](https://github.com/AdvancedDiscordBot/adb-plugin-template#pluginjson-fields).

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | **Yes** | Internal name — must equal the npm package name and start with `adb-plugin-` |
| `displayName` | string | **Yes** | Human-readable name shown in the marketplace |
| `description` | string | **Yes** | What the plugin does |
| `author` | string | **Yes** | Developer / publisher name |
| `version` | string | **Yes** | Current published version (semver) |
| `category` | string | **Yes** | One of: `moderation`, `utility`, `analytics`, `entertainment`, `core features` |
| `npmPackage` | string | **Yes** | Exact npm package name (installs run `npm install <this>`) |
| `main` | string | No | Entry file, defaults to `index.js` |
| `permissions` | array | No | Capability strings the plugin needs (e.g. `db.read`, `db.write`, `commands.register`) |
| `requiresRestart` | boolean | No | `true` if applying it needs a bot restart (disables hot-reload) |
| `verified` | boolean | No | Set by maintainers — official / audited plugin |
| `configSchema` | object | No | JSON Schema that renders the per-guild settings UI |

> These keys match what you see in [`plugins.json`](./plugins.json) today — copy an existing entry as your starting point.

### Add your entry

Fork the repo and add your plugin to the `plugins` array in [`plugins.json`](./plugins.json):

```json
{
  "plugins": [
    {
      "name": "adb-plugin-reminders",
      "displayName": "Reminders",
      "description": "Adds /remind to set, list, and cancel personal reminders. Delivers via DM with a channel fallback.",
      "author": "DeadIndian",
      "version": "1.3.0",
      "category": "utility",
      "npmPackage": "adb-plugin-reminders",
      "main": "index.js",
      "requiresRestart": false,
      "verified": true,
      "permissions": ["db.read", "db.write", "commands.register"],
      "configSchema": {
        "type": "object",
        "properties": {
          "maxPerUser": { "type": "number", "default": 25, "minimum": 1, "maximum": 200 }
        }
      }
    }

    // 👉 add your plugin object here
  ]
}
```

`verified` is set by maintainers during review — leave it out or `false` in your PR.

### Open a PR

Commit, push your fork, and open a pull request against `main`. That's it — wait for review.

---

## ✅ Plugin Requirements

- Published to **npm** with a package name starting `adb-plugin-`.
- Ships a valid `plugin.json` manifest (see the [template](https://github.com/AdvancedDiscordBot/adb-plugin-template)).
- Follows the ADB plugin contract — exports `async load(ctx)` and stays isolation-safe.
- Declares every capability it uses; must not attempt to escape the sandbox.
- Must not break the bot.

## 🔎 Review Process

Before merging, maintainers check:

- The `plugins.json` entry is valid JSON with all required fields.
- The npm package installs and loads cleanly in a real bot.
- The declared `permissions` / capabilities match what the code actually does.
- No security issues — no sandbox-escape attempts, no undisclosed network/filesystem access, no obfuscated code.

---

## 📄 License

This registry is licensed under the **GNU Affero General Public License v3.0** — see [LICENSE](./LICENSE). It follows the policies of the main ADB project:

- **Contributing**: [CONTRIBUTING.md](./CONTRIBUTING.md)
- **Code of Conduct**: [CODE_OF_CONDUCT.md](https://github.com/AdvancedDiscordBot/Advanced-Discord-Bot/blob/main/CODE_OF_CONDUCT.md)
- **Security Policy**: [SECURITY.md](https://github.com/AdvancedDiscordBot/Advanced-Discord-Bot/blob/main/SECURITY.md)
