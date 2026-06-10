```json
{
  "autoMemoryDirectory": "__VAULT_ABS_PATH__/Memory"
}
```

> Setup step: write this to `./.claude/settings.local.json` (NOT `settings.json`), then replace `__VAULT_ABS_PATH__` with the absolute path of the vault root (the current working directory), so Claude auto-memory is redirected into `./Memory/`. No symlink needed. Verify the path resolves before continuing.
>
> Windows: use FORWARD slashes in the path (`C:/Users/name/SoloHQ/Memory`, not `C:\Users\...`). Backslashes are escape characters in JSON and will corrupt the setting or the whole file. Claude Code accepts forward-slash paths on Windows.
>
> Why `settings.local.json` and not `settings.json`: `autoMemoryDirectory` is only honored from **local** (or user) settings scope, not from project-scope `settings.json`. A project `settings.json` can travel with a cloned repo, so the harness deliberately ignores a memory redirect coming from it (a repo should not be able to repoint your memory). Verified hands-on 2026-06-04: the same setting was silently ignored from `settings.json` (native memory fell back to the default `~/.claude/projects/<slug>/memory` hash path) but honored from `settings.local.json`, on both the terminal CLI and the desktop app. An absolute, per-machine path also belongs in local scope, so this is its correct home regardless.
