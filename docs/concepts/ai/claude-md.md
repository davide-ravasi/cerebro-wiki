---
id: concept-claude-md
title: "CLAUDE.md (project memory for AI coding agents)"
type: concept
domain: ai
tags: [concept, ai, tooling, developer-experience]
status: evergreen
confidence: medium
updated: 2026-06-23
---

# Definition

`CLAUDE.md` is a Markdown file placed at the root of a repository that gives an AI
coding agent (Claude Code) persistent, project-specific context. It is read
automatically at the start of every session and injected into the model's context.

# Why It Matters

Without it, the agent rediscovers the same facts every session — project structure,
build/test commands, conventions, known pitfalls. `CLAUDE.md` turns that into shared
**project memory**: committed to the repo, it benefits the whole team and makes the
agent productive from the first message instead of after exploration.

# How It Works

- Lives at the repository root as plain Markdown; no activation step is needed.
- Loaded at session start and prepended to the working context — it is always
  "in the prompt", so it costs context tokens on every turn.
- Acts as instructions/context, not executable config: it describes *what* the agent
  should know (commands, layout, conventions), not automated behaviours.
- Resolution is hierarchical: a root file for the whole repo, and optional
  `CLAUDE.md` files in subdirectories for local context.

# Tradeoffs

- Pros: zero-setup recall, team-shareable, reduces repeated exploration, encodes
  tribal knowledge (gotchas, commands) in one discoverable place.
- Cons: always-loaded → must stay concise or it wastes context budget; goes stale if
  not maintained; not a substitute for real config or automation.

# When To Use

- Use when a repo has non-obvious structure, commands, or conventions worth capturing
  once for both humans and the agent.
- Keep it factual and short: commands, paths, architecture summary, pitfalls.
- Avoid dumping long prose or duplicating what the code/README already makes obvious.

# Example

A typical root `CLAUDE.md` contains: a one-paragraph "what this is", a repo layout
tree, the tech stack, a commands table (dev / build / test / lint), a short
"how it works" flow, required environment variables, and a "gotchas" list. Variants:

- `CLAUDE.md` — committed, shared with the team.
- `CLAUDE.local.md` — personal, not committed (machine-specific notes).
- Per-subdirectory `CLAUDE.md` — scoped context for a specific package or area.

# Related Concepts

- [[concept-retrieval-augmented-context]]
- [[concept-prompt-context-window]]

# Related Patterns

- [[pattern-documenting-a-repo-for-ai]]
