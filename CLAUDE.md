# SoloHQ

The free foundation of the **Sovereign Agency OS**: a local AI operating system that knows your voice, your business, and your goals. Distributed as a small Claude Code plugin marketplace.

## What This Is

This repo is the SoloHQ marketplace. It ships two free plugins:

| Plugin | Skills | Purpose |
|--------|--------|---------|
| **core** | `os-setup`, `ai-identity-blueprint`, `business-and-story-map` | Bootstraps your vault (folders, `CLAUDE.md`, memory system), then runs the two-part identity onboarding: Part 1 (Find Your Voice) and Part 2 (Map Your Business & Story). |
| **agentic-os** | `agentic-os-obsidian` | Installs a command-center dashboard inside your Obsidian vault (5 bundled plugins, Home + per-profile + Vault Overview pages, button bar wired to Claude prompts). |

## Install (for members)

**Claude desktop app (the path most members use):** Customize → Plugins → under Personal plugins click **+** → **Add marketplace** → paste `https://github.com/successt/solohq` → install **core** and **agentic-os** with the **+** next to each. Watch out for Anthropic's similar-sounding tiles (`setup-cowork`, `cowork-plugin-management`); the SoloHQ plugins are the two under the marketplace you just added.

**Claude Code (terminal):**

```
/plugin marketplace add successt/solohq
/plugin install core@solohq
/plugin install agentic-os@solohq
```

Then run `/os-setup` to bootstrap your vault, and say `build my blueprint` to capture your voice.

**Updating:** marketplaces pin to the commit they were added at. Reinstalling a plugin alone serves the stale pinned version; remove and re-add the marketplace (desktop) or `/plugin marketplace update solohq` (Claude Code) to pull latest.

## Repo Layout

```
.claude-plugin/
  marketplace.json   - the two published plugins
  skills-map.json    - which skills belong to which plugin
shared-skills/       - single source of truth for every skill
plugins/             - GENERATED from shared-skills/ (do not edit by hand)
sync-skills.sh       - regenerates plugins/ from shared-skills/ + skills-map.json
build-zips.sh        - builds installable zips into dist/
```

## Development

All skills live in `shared-skills/` as the single source of truth. `plugins/*/skills/` is generated, never edit it directly (sync overwrites it).

Editing workflow:
1. Edit the skill in `shared-skills/<skill>/`
2. If adding a skill or changing plugin membership, update `.claude-plugin/skills-map.json`
3. Run `./sync-skills.sh`

Before every push, run `./sync-skills.sh` so `plugins/` matches `shared-skills/` + `skills-map.json`.

## Building Distributable Zips

Run `./build-zips.sh` to generate zips in `dist/`. It runs `sync-skills.sh` first, reads `marketplace.json`, and emits one zip per plugin plus a full marketplace zip.

## License

MIT. See `LICENSE` for full copyright and attribution.
