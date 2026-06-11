# claude-stuff

Versioned [Claude Code](https://claude.com/claude-code) configuration for the **IntegratedDynamic** platform — **subagents** and **skills**, shared across all repos (`infrastructure`, `gitops`, …) and loaded user-level via symlinks.

- **Agents** ([`agents/`](agents/)) — single Markdown files with YAML frontmatter (`name`, `description`, `tools`, `model`) + the agent's system prompt. Spawned by Claude to run a focused task autonomously.
- **Skills** ([`skills/`](skills/)) — `<name>/SKILL.md` with frontmatter (`name`, `description`) + a playbook. Run **in the main conversation**, so they're the right home for interactive, back-and-forth workflows (clarify → iterate → act).

Rule of thumb: a task that needs dialogue with the human → **skill**; a task you hand off to run on its own → **agent**.

## Agents

- **`devops`** — DevOps/IaC + GitOps engineer. Detects whether it's operating in the `infrastructure` repo (Terraform bootstrap, mise, GitHub Actions) or the `gitops` repo (ArgoCD-reconciled Helm state) and applies the matching playbook, tooling, and "what counts as a deploy". Consumes `po`-authored issues.

## Skills

- **`po`** — Product Owner workflow. Turns a request (one-liner → detailed brief) into a self-contained GitHub issue a dev agent can execute. Clarifies interactively, drafts a spec with [OpenSpec](https://github.com/Fission-AI/OpenSpec) (`proposal`/`specs`/`design`/`tasks`) in a local gitignored `openspec/` workspace, then synthesizes that into the issue and routes it to a dev agent via an `agent:<name>` label. The OpenSpec workspace is disposable scratch — the **issue** is the durable artifact.

## Install (one-time, per machine)

Clone this repo and symlink both trees into your user-level Claude config so agents *and* skills load in **every** repo:

```bash
git clone git@github.com:IntegratedDynamic/claude-stuff.git ~/IntegratedDynamic/claude-stuff
ln -s ~/IntegratedDynamic/claude-stuff/agents ~/.claude/agents
ln -s ~/IntegratedDynamic/claude-stuff/skills ~/.claude/skills
```

User-level agents/skills are overridden by a project's own same-named `.claude/agents/` or `.claude/skills/` entry, so don't commit a same-named one into an individual repo — edit it here instead.

## Editing

Edit the file under `agents/` or `skills/`, commit, and push. Changes take effect in new Claude Code sessions immediately (the symlinks point straight at the working tree — no reinstall needed).
