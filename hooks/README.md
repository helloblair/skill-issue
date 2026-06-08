# Hooks

Hooks are configured in Claude Code's `settings.json` (the harness runs them, not
the model), so they fire mechanically and reliably. Unlike skills, they cannot be
symlinked into `~/.claude/skills/`. You register them by pointing your settings at
the script in this folder.

## activity-log.py

The automatic "what changed" feed. On every file edit/create Claude makes, it
appends one terse, timestamped line to `<project>/docs/activity-log.md`, and it
drops a marker line whenever a `git commit` runs. It is the reliable companion to
the on-demand [`doc-logging`](../skills/doc-logging/SKILL.md) skill (which writes
the rich "why" entries and the codebase map that a script cannot).

Example output:

```
## 2026-06-08

14:32  modified  src/api/route.ts   (+12/-3)
14:33  wrote     src/lib/embed.ts   (+47/-0)
  -- committed: "feat(api): add embed route" --
14:41  modified  package.json       (+1/-0)
```

### What it does and does not capture

- Captures changes made **through Claude Code's editor tools** (Edit, Write,
  NotebookEdit). It does NOT see edits you type by hand in your editor. For that
  you would need a git hook or a file watcher instead.
- Logs file **paths and line counts only**, never file contents, so secrets never
  land in the log.
- Skips noise: the log file itself, lockfiles, `node_modules/`, `dist/`, `build/`,
  `.next/`, `target/`, `__pycache__/`.
- Keeps the log invisible to git in **every** repo by adding it to
  `.git/info/exclude` (a local, never-committed ignore file), so it never pollutes
  a repo you do not own.
- Is fail-safe: any error is swallowed and it always exits 0, so it can never
  break or slow down an edit.

### Requirements

- `python3` on PATH (preinstalled on macOS).
- This repo cloned at `~/skill-issue` (the settings snippet below points there). If
  you clone it elsewhere, update the path.

### Install (global, every project)

Add this to `~/.claude/settings.json` (or `~/.claude/settings.local.json`):

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write|NotebookEdit|Bash",
        "hooks": [
          { "type": "command", "command": "python3 \"$HOME/skill-issue/hooks/activity-log.py\"" }
        ]
      }
    ]
  }
}
```

### Install (one project only)

Put the same `hooks` block in that repo's `.claude/settings.local.json` instead of
the global file. It will then log only that project.

### Activating after install

Claude Code captures hooks at startup. After editing settings, open the `/hooks`
menu to review and load the new hook, or start a new session. This review step is
a security feature so hooks cannot change underneath you silently.
