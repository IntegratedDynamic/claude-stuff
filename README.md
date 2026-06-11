# claude-agents

Versioned [Claude Code](https://claude.com/claude-code) subagent definitions for the **IntegratedDynamic** platform, shared across all repos (`infrastructure`, `gitops`, …).

Agents live under [`agents/`](agents/). Each is a single Markdown file with YAML frontmatter (`name`, `description`, `tools`, `model`) followed by the agent's system prompt.

## Agents

- **`po`** — Product Owner. Turns a request (one-liner → detailed brief) into a self-contained GitHub issue a dev agent can execute. Uses [OpenSpec](https://github.com/Fission-AI/OpenSpec) (spec-driven development) to draft `proposal`/`specs`/`design`/`tasks` locally (in a gitignored `openspec/` workspace), then synthesizes that into the issue and routes it to the right dev agent via an `agent:<name>` label. The OpenSpec workspace is disposable scratch — the **issue** is the durable artifact.
- **`devops`** — DevOps/IaC + GitOps engineer. Detects whether it's operating in the `infrastructure` repo (Terraform bootstrap, mise, GitHub Actions) or the `gitops` repo (ArgoCD-reconciled Helm state) and applies the matching playbook, tooling, and "what counts as a deploy". Consumes `po`-authored issues.

## Install (one-time, per machine)

Clone this repo and symlink it into your user-level Claude config so the agents load in **every** repo:

```bash
git clone git@github.com:IntegratedDynamic/claude-agents.git ~/IntegratedDynamic/claude-agents
ln -s ~/IntegratedDynamic/claude-agents/agents ~/.claude/agents
```

User-level agents are overridden by a project's own `.claude/agents/<name>.md` of the same name, so don't commit a same-named agent into an individual repo — edit it here instead.

## Editing

Edit the file under `agents/`, commit, and push. Changes take effect in new Claude Code sessions immediately (the symlink points straight at the working tree — no reinstall needed).
