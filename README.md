# tea-cli-skill

A Codex skill for working with Gitea through the [`tea`](https://gitea.com/gitea/tea) CLI.

## What this skill does

The `tea-cli` skill teaches an agent how to interact with Gitea repositories from the terminal using `tea` instead of the web UI.

It covers:

- **Authentication** — checking the active account with `tea whoami` and `tea logins list`
- **Issues** — listing, showing, creating, editing, closing, and reopening issues
- **Pull Requests** — listing, showing, creating, approving, rejecting, merging, and checking out pull requests
- **Comments** — adding follow-up discussion to issues and pull requests
- **Repo context** — inferring owner/repo from `$PWD`, or pinning it explicitly with `--repo`, `--login`, and `--remote`

## Skill files

| Path | Purpose |
|------|---------|
| `skills/tea-cli/SKILL.md` | Core skill: purpose, quick-start, workflows, context rules, hard rules, and recovery steps |

## Usage

Add this repository as a skill source in your Codex or agent configuration. Once installed, the agent will consult `skills/tea-cli` whenever a task involves Gitea issues, pull requests, or comments via `tea`.
