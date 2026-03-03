<p align="center">
  <img src="assets/banner.svg" alt="Skill Issue" width="800"/>
</p>

A personal library of reusable [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skills and commands — portable across any project.

## What's in here?

This repo contains two types of Claude Code extensions:

- **Skills** — Passive reference files that Claude reads for context and patterns. They inform how Claude approaches specific tasks (deploying, documenting, building pipelines) without you having to re-explain every time.
- **Commands** — User-invocable actions triggered with `/project:<command>`. These execute a specific workflow on demand.

## Skills

### doc-logging

Automatically maintains living documentation after every meaningful code change. Creates and appends to two local, gitignored files: a **sprint changelog** (what changed, why, and what it unlocks) and a **codebase audit** (a component-by-component snapshot of the project's current state). Designed to serve as a private developer reference, debugging history, and interview prep material.

### rag-pipeline

A reference guide and pattern library for building Retrieval-Augmented Generation pipelines from scratch. Covers the full flow: file discovery, chunking strategies (with language-specific examples for COBOL, Python, JS/TS, C, and more), embedding generation with OpenAI, vector storage in Pinecone, semantic search, and LLM answer generation with source citations. Includes a troubleshooting catalog for common failure modes like irrelevant retrieval, dimension mismatches, and hallucination.

### vercel-deploy

A deployment checklist and troubleshooting guide for shipping Next.js applications to Vercel. Covers the deploy-first philosophy, initial project setup, environment variable configuration, API route patterns, serverless function timeout mitigation, and the standard dev-to-production workflow. Includes common gotchas and their fixes.

## Commands

### takeaway

A manually triggered learning journal (`/project:takeaway [topic]`). When invoked, it reviews the current conversation context, reconstructs what just happened, and appends a detailed step-by-step explanation to `takeaways.md` using real file names, function names, and numbers from the session. Written in plain language for a future reader who forgot the context. Doubles as study notes and interview prep.

## Usage

### Import everything into a project

From your project root:

```bash
# Link both skills and commands
ln -s ~/skill-issue/skills/* .claude/skills/
ln -s ~/skill-issue/commands/* .claude/skills/
```

### Import only skills or only commands

```bash
# Skills only
mkdir -p .claude/skills
ln -s ~/skill-issue/skills/* .claude/skills/

# Commands only
mkdir -p .claude/skills
ln -s ~/skill-issue/commands/* .claude/skills/
```

### Pick and choose individual items

```bash
mkdir -p .claude/skills

# Just the ones you want
ln -s ~/skill-issue/skills/doc-logging.md .claude/skills/doc-logging.md
ln -s ~/skill-issue/skills/vercel-deploy.md .claude/skills/vercel-deploy.md
ln -s ~/skill-issue/commands/takeaway .claude/skills/takeaway
```

### Adding new skills

Drop a new `.md` file into `skills/` (for passive skills) or a new subfolder with a `SKILL.md` into `commands/` (for invocable commands), and update this README. Any project linked to the full directory will pick it up automatically.
