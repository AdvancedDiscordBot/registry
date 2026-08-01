# Contributing to the ADB Plugin Registry

Thanks for helping grow the Advanced Discord Bot ecosystem. This repo is **just an index** — a single [`plugins.json`](./plugins.json) file that every ADB instance reads to populate its in-dashboard marketplace. Contributing here means listing a plugin you've already published to npm.

If you're looking to build the plugin itself, start with the [`adb-plugin-template`](https://github.com/AdvancedDiscordBot/adb-plugin-template) and its README — that's the developer guide. This document only covers getting a finished plugin **listed**.

## Before you open a PR

Your plugin must already be:

- **Published to npm** under a package name that starts with `adb-plugin-`.
- Shipping a valid `plugin.json` manifest and exporting `async load(ctx)`.
- **Isolation-safe** — it runs in a sandboxed worker and only reaches Discord / the database / the scheduler through capability-gated `ctx` calls it has declared. See the [template README](https://github.com/AdvancedDiscordBot/adb-plugin-template#isolation-read-this-first).
- Tested in a real bot (see "Testing inside a real bot" in the template README).

## Adding your entry

1. **Fork** this repo and create a branch.
2. Add one object to the `plugins` array in [`plugins.json`](./plugins.json). Copy an existing entry as your starting point and keep the array valid JSON (no trailing commas). See the [Plugin Entry Reference](./README.md#plugin-entry-reference) for every field.
3. Fill in the required fields honestly:
   - `name` and `npmPackage` must be identical and match your published package.
   - `permissions` must list **only** what your code actually uses — reviewers check this against the source.
   - Leave `verified` out (or `false`). Maintainers set `verified: true`, not submitters.
4. Validate your JSON before pushing:
   ```bash
   node -e "JSON.parse(require('fs').readFileSync('plugins.json','utf8')); console.log('valid JSON')"
   ```
5. **Open a PR** against `main` with a short description of what the plugin does.

## What maintainers review

- `plugins.json` is valid and your entry has all required fields.
- The npm package installs and loads cleanly in a real bot.
- Declared `permissions` / capabilities match the actual behavior.
- No security issues — no sandbox-escape attempts, no undisclosed network or filesystem access, no obfuscated code.

## Updating or removing a listing

- **New version:** bump the `version` (and any changed `configSchema` / `permissions`) in your entry and open a PR. Users get the update on their next marketplace refresh.
- **Removal:** open a PR deleting your entry, or open an issue if you can't.

## Reporting a problem

Found a listed plugin that misbehaves or over-reaches its declared permissions? Open an issue here or follow the [Security Policy](https://github.com/AdvancedDiscordBot/Advanced-Discord-Bot/blob/main/SECURITY.md) for anything sensitive.

By contributing you agree your submission is licensed under [AGPL-3.0](./LICENSE), consistent with the rest of the ADB project. All participants are expected to follow the [Code of Conduct](https://github.com/AdvancedDiscordBot/Advanced-Discord-Bot/blob/main/CODE_OF_CONDUCT.md).
