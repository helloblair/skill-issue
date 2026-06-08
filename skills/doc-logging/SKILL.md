---
name: doc-logging
description: Write rich, deliberate project documentation on demand. Maintains a sprint changelog (what changed, WHY, the tradeoffs, what it unlocks) and a component-by-component codebase audit (the living map of the project's current state). Use when the user runs /doc-logging, asks to write up recent changes, wants the "why" behind a change recorded, or wants the codebase map refreshed. This is the deep-dive companion to the automatic activity-log hook: the hook records the terse "what changed" feed for free; this skill captures the reasoning and the big-picture map that a script cannot.
---

# Documentation Logging Skill

## Purpose

Maintain a living record of the engineering decisions and the system's current state, so the developer can always answer "why did I do this?" and "what does the project look like right now?" These logs serve as private developer reference, interview prep material, and debugging history.

This skill is the **deliberate, deep-dive half** of a two-part system:

- The **activity-log hook** (configured in settings.json) automatically appends a terse one-line breadcrumb per file change to `docs/ACTIVITY_LOG.md`. That is the always-on "what changed" feed, and it runs with no model involved.
- **This skill** writes the parts a script cannot: the *why* behind a change and the component-by-component map. Invoke it at natural breakpoints (a finished feature, a tricky fix, the end of a session), not on every keystroke.

## What To Do When Invoked

When the user invokes this skill, APPEND to both files below, covering the most recent meaningful set of changes. These are append-only logs — never overwrite or delete existing content.

If the user named a specific change or topic, scope the entry to that. Otherwise, review the recent conversation and the activity log to reconstruct what was done.

These files are LOCAL-ONLY and gitignored. Do not `git add` them or include them in commits. They exist solely for the developer's private reference.

## File 1: docs/CHANGELOG_SPRINT.md

Append an entry in this exact format:

```markdown
---

### [SHORT_TITLE_OF_CHANGE]
**Timestamp:** [YYYY-MM-DD HH:MM UTC]
**Commit:** `[commit message using conventional format]`
**Files Changed:** [list of files added/modified/deleted]

**What Changed:**
[2-3 sentences on what was built, fixed, or modified]

**Engineering Rationale:**
[Why this approach was chosen. What tradeoffs were considered. What was rejected and why. Be specific — this is interview prep material.]

**Impact:**
[What this unlocks, fixes, or unblocks]
```

## File 2: docs/CODEBASE_AUDIT.md

Append a new section or update an existing one. Organized by component:

```markdown
## [COMPONENT_NAME] (updated [YYYY-MM-DD])

**Location:** [file paths]
**Purpose:** [what it does in plain language]
**Dependencies:** [what it imports/requires/calls]
**Exposes:** [what other components use from it]
**Status:** [working | in-progress | stubbed | broken]
**Notes:** [gotchas, known issues, TODOs, edge cases discovered]
```

If a component already has a section, UPDATE it in place. Do not create duplicate sections.

## Commit Messages

Use conventional commits: `type(scope): description`

Types: `feat`, `fix`, `docs`, `test`, `refactor`, `chore`, `build`, `deploy`

Scopes should match your project's architecture (e.g., `api`, `frontend`, `auth`, `db`, `deploy`, `docs`).

## When NOT to Write a Deep Entry

The automatic activity-log hook already captures every change, so this skill is reserved for changes worth explaining. Skip a deep entry for:

- Typo or formatting-only fixes
- Changes to the log files themselves
- `.env` or secret-related changes
- Running scripts without modifying code

## Setup

On first use, create both files if they don't exist:

```bash
mkdir -p docs
touch docs/CHANGELOG_SPRINT.md docs/CODEBASE_AUDIT.md
```

Add to `.gitignore` (the activity-log hook writes a third file in the same folder):

```
docs/CHANGELOG_SPRINT.md
docs/CODEBASE_AUDIT.md
docs/ACTIVITY_LOG.md
```
