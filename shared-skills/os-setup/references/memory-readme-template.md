---
type: system
tags: [memory, protocol, system]
---

%% Portable memory protocol for a member vault. Any AI working in this vault reads this first. Plain markdown so any AI platform can use it, not just Claude. %%

> [!important] What this folder is
> `Memory/` is your assistant's brain. It is plain markdown that lives in your own vault, so you own it, you can read it, and it travels with your folder. Whichever AI surface you use, your assistant reads and writes here. **This folder is the single source of truth for what your assistant remembers about you.**

## How memory works here

1. **`MEMORY.md` is the index.** The assistant reads it at the start of every session. It is a list of one-line pointers to entry files. Loading it gives cross-session recall without reading every file.
2. **Each fact is its own entry file.** One topic per file, standalone and self-contained. Naming convention by prefix:
   - `feedback_*` how you want the assistant to behave (corrections, preferences, do/don't).
   - `project_*` live state of something you are working on (status, what's done, what's next).
   - `reference_*` durable facts and details worth keeping (how a tool behaves, a useful detail learned).
3. **To remember something new:** create a new entry file with the right prefix, then add a one-line pointer to it in `MEMORY.md`. Keep entries short and skimmable.
4. **To update a fact:** edit the entry file in place and keep its `MEMORY.md` pointer accurate.
5. **Keep the index honest.** If an entry is removed or merged, fix its line in `MEMORY.md`. No orphan pointers, no orphan files.

## The two surfaces and the drop-box

You may talk to your assistant in more than one place (for example a coding surface and an assistant/Cowork surface). They write automatic memory to different spots, but **`Memory/` is always the one true home.** Here is how they are kept in sync:

- The coding surface is pointed at this `Memory/` folder directly (via the `autoMemoryDirectory` setting), so its memory lands here.
- The assistant/Cowork surface automatically writes a single `memory.md` file at the **root of your vault**. Treat that file as a **drop-box (an inbox), not a second memory.** It is temporary.

> [!important] The sweep (the assistant runs this at the start of every session)
> 1. Read `Memory/MEMORY.md`. If it is missing, say so out loud to the user and continue without pretending to recall anything. Never fabricate memory from a missing index.
> 2. Look for a `memory.md` file at the vault root. If it exists and has real content (more than the sentinel line below):
>    - For each note in it, check whether it is already captured in `Memory/`. Skip anything already there (dedupe).
>    - Fold each genuinely new note into the right `Memory/` entry file (by topic and prefix), or create a new entry, and update the `MEMORY.md` pointer.
>    - After folding, **clear `memory.md` back to the sentinel**: a file containing only `# Memory` and the comment `<!-- swept into Memory/ -->`. This keeps the drop-box from ever piling up, and works whether the surface overwrites or appends it next time.
>    - Tell the user in one line what was swept in.
> 3. Proceed with the session.

## Rules

- **No secrets.** Never store API keys, tokens, or passwords here. Reference their location instead (for example "token is in my password manager"). This folder is plain text.
- **Confirm saves out loud.** Whenever the assistant saves or updates a memory, it says so in one short line (for example "Saved to `Memory/`: your preferred tone"). You should always be able to see your memory growing.
- **Consolidate at scale.** When `MEMORY.md` passes about 35 entries, do a consolidation pass: merge related entries, retire stale ones, tighten the index so it stays skimmable. A bloated index gets skipped at the bottom and quietly fails.
- **Stay flat.** Entry files sit directly next to `MEMORY.md`. Do not nest them into subfolders.
- **Memory vs. the rest of the vault.** This folder is condensed cross-session recall (the index card). The fuller record lives in your other vault folders. Short operational facts the assistant needs at startup go here; durable detail goes to the matching vault folder.

## For non-Claude AIs

If you are an AI other than Claude working in this vault: point yourself at `Memory/MEMORY.md` as your persistent memory index, read it before acting, run the sweep above, and follow the protocol to add or update entries. This is how every AI that works here shares one memory.
