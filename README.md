# tea-cli-skill

This repository provides a Codex skill for working with Gitea from the terminal with
[`tea`](https://gitea.com/gitea/tea).

## What the skill is for

Use the `tea-cli` skill when a task should be handled with `tea` instead of the Gitea
web UI. The skill is aimed at the common repository workflows an agent needs most often:

- checking which Gitea account is active
- working with issues
- working with pull requests
- adding follow-up comments
- selecting the right repo, login, or remote when context is ambiguous

## What it teaches

The skill focuses on high-level `tea` workflows, including:

- `tea whoami` and `tea logins list` for authentication checks
- `tea issues ...` for issue listing, inspection, creation, editing, closing, and reopening
- `tea pulls ...` and `tea pr ...` for pull request listing, review, merge, and checkout flows
- `tea comment` for issue and pull request discussion
- `--repo`, `--login`, and `--remote` for explicit repo targeting
- `--output json` when command output needs to be parsed by follow-up automation

## Example tasks this skill should help with

- "List open issues in this repo"
- "Create an issue describing this bug"
- "Show pull request 14 as JSON"
- "Approve and merge the current PR"
- "Comment on issue 12 from another repo using `--repo`"

## Repository layout

| Path | Purpose |
|------|---------|
| `skills/tea-cli/SKILL.md` | Main skill instructions, workflows, rules, and recovery guidance |
| `skills/tea-cli/agents/openai.yaml` | Agent metadata and default prompt for the skill |

## Using the skill

Add this repository as a skill source in your Codex or agent configuration. Once installed,
the agent should consult `skills/tea-cli` whenever a task involves Gitea issue, pull request,
comment, or repo-selection work through `tea`.
