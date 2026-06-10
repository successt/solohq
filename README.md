# SoloHQ

**The command center for a one-person business.** A local AI operating system that actually knows you, part of the Sovereign Agency OS and the free foundation of the 7-Figure Path community.

## What it is

SoloHQ is an AI operating system that runs on your own machine and knows you. Not a chatbot you re-explain yourself to every morning, and not another tool that rents you access to your own work. It lives in plain files you own, it holds your voice, your business, your clients, and your goals in a memory that persists across every session, and it gets smarter about you the more you use it. Because it runs locally and everything is yours, you are never dependent on one platform, one login, or one company's roadmap.

## What it does

It works alongside you instead of waiting for prompts. It reads and writes your notes, routes information to the right place, drafts in your actual voice, tracks your projects and goals, and executes real tasks across your systems. The result is leverage that compounds: you build the system once, and it keeps paying you back, quietly running the operating layer of a one-person business that used to take a whole team.

## Install

You need an AI client that can read files and run skills. SoloHQ works with the **Claude desktop app** (the easiest path for most people) and **Claude Code** (the terminal version, for technical users). Mac or PC. A paid Claude plan is required to run plugins.

SoloHQ is two free plugins: `core` sets up your vault, memory, and voice. `agentic-os` adds the Obsidian command-center dashboard. Install both.

### Claude desktop app (recommended)

1. Open the Claude desktop app.
2. Go to **Customize → Plugins**.
3. Under **Personal plugins**, click **+** and choose **Add marketplace**.
4. Paste this URL and confirm:

   ```
   https://github.com/successt/solohq
   ```

5. The SoloHQ marketplace appears with two plugins, **core** and **agentic-os**. Click the **+** next to each one to install both.
6. Start a new conversation and type `/os-setup` (or just say "set up SoloHQ").

> **Install only from the SoloHQ list you just added.** The plugin browser also shows Anthropic's own tiles with similar-sounding names (like `setup-cowork` and `cowork-plugin-management`). Those are not SoloHQ. The two you want are **core** and **agentic-os**, listed under the marketplace you added in step 4.

### Claude Code (terminal)

```
/plugin marketplace add successt/solohq
/plugin install core@solohq
/plugin install agentic-os@solohq
```

Then run `/os-setup`.

### Updating SoloHQ

Marketplaces pin to the version that existed when you added them, so reinstalling a plugin by itself will NOT pull the latest release. To update:

1. Go to **Customize → Plugins**, remove the SoloHQ marketplace.
2. Add it back (steps 3 to 5 above) and reinstall **core** and **agentic-os**.

In Claude Code: `/plugin marketplace update solohq`, or remove and re-add the marketplace.

## What happens when you set it up

1. **Setup builds your vault.** It creates your folder structure, your memory system, and your assistant's instructions, all as plain markdown in a folder you own. Your assistant's memory is wired in automatically, no technical steps. When you want the visual command-center dashboard, say "set up agentic OS in Obsidian" and the `agentic-os` plugin builds it inside your vault.
2. **Part 1, Find Your Voice.** A short interview captures how you actually communicate, so your assistant writes like you, not like generic AI. This is the moment it stops feeling like a tool and starts feeling like it knows you. Just say "build my blueprint."
3. **Part 2, Map Your Business & Story.** A deeper, reflective pass that maps what you have built, where you came from, and the honest gaps holding you back, then hands you a real fix in your own voice. Come back to it whenever you are ready by saying "map my business." Also free.

That is the free foundation: your assistant knows your voice, your business, and your story. The goal-getting app and the deeper membership builders live inside the 7-Figure Path.

## Your data is yours

Everything SoloHQ knows about you lives in plain markdown files in your own folder. You can read it, back it up, move it to another machine, or walk away with it. Nothing is locked in someone else's cloud. By default your memory, identity, and client work are kept out of any public repo.

## Works with

Claude Code, the Claude desktop app (Cowork), and other file-capable AI agents that read your project instructions.

## License

MIT licensed. SoloHQ builds on an open-source MIT-licensed scaffold, with the SoloHQ operating system, memory design, and AI Identity Blueprint added on top. See `LICENSE` for full copyright and attribution.
