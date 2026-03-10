---
name: tea-cli
description: Use this skill when working with Gitea through the `tea` CLI, especially for repository-scoped issue, pull request, login, comment, merge, and API fallback workflows from a terminal.
---

# Tea CLI

## Purpose

Use this skill when the task is to interact with Gitea from the terminal with `tea` instead of the web UI.

Typical triggers:

- create, inspect, update, or close issues
- create, inspect, review, approve, reject, or merge pull requests
- comment on issues or pull requests
- work against a repo other than the current directory with `--repo`
- fall back to `tea api` when a higher-level `tea` subcommand does not expose the field or action you need

## Quick Start

1. Confirm authentication and active account:
   - `tea whoami`
   - `tea logins list`
2. Prefer running inside the target git repository so `tea` can infer owner, repo, and login from `$PWD`.
3. If repo inference is wrong or unavailable, pass one or more of:
   - `--repo owner/repo`
   - `--login <login-name>`
   - `--remote <remote-name>`
4. Before `tea pr create`, make sure the source branch is committed and pushed. `tea` assumes the git state is already published.
5. Prefer explicit non-interactive flags such as `--title`, `--description`, `--base`, and `--head`.
6. When the result must be parsed or verified, add `--output json`.

## Context Rules

- `tea` is repo-aware. Default to the current repository context first.
- Use `--repo` when operating outside a checkout or when the current checkout is not the target repo.
- Use `--login` when more than one Gitea account is configured.
- Use `--remote` when login selection should come from a specific git remote.
- Prefer high-level `tea` subcommands first. Use `tea api` only for gaps or unsupported flows.

## Core Workflows

### Issues

- List issues:
  - `tea issues list`
  - `tea issues --state all --output json`
- Show one issue:
  - `tea issues 12`
  - `tea issues 12 --output json`
- Create an issue:
  - `tea issues create --title "Bug: session leak" --description "Steps, expected result, actual result"`
- Update or close an issue:
  - `tea issues edit 12 --title "Updated title"`
  - `tea issues close 12`
  - `tea issues reopen 12`

### Pull Requests

- List pull requests:
  - `tea pulls list`
  - `tea pulls --state all --output json`
- Show one pull request:
  - `tea pulls 4`
  - `tea pulls 4 --output json`
- Create a pull request:
  - `tea pr create --head feature/branch --base main --title "fix: handle login fallback" --description "Closes #12"`
- Approve, reject, or merge:
  - `tea pr approve 4`
  - `tea pr reject 4`
  - `tea pr merge 4`
- Check out an existing pull request locally:
  - `tea pr checkout 4`

### Comments

- Use `tea comment` for issue or PR follow-up discussion.
- Keep comments explicit and non-interactive when automating.
- If the exact flags are needed for the installed version, inspect `tea comment --help` before sending the command.

## API Fallback Pattern

Use `tea api` when:

- a high-level subcommand is missing a required field
- you need a raw endpoint that `tea` does not wrap cleanly
- you want exact API payload control

Rules:

1. Prefer endpoint paths with `{owner}` and `{repo}` placeholders so repo context is reused.
2. Use `-f` for string fields.
3. Use `-F` for typed values such as booleans, numbers, `null`, or file/stdin payloads.
4. Use `-X` to set the method explicitly for writes.
5. Keep responses machine-readable when the output will be consumed downstream.

Read [references/api-fallbacks.md](references/api-fallbacks.md) when you need common endpoint patterns.

## Hard Rules

1. Prefer non-interactive invocations in agent workflows.
2. Do not assume repo context if the current directory is ambiguous; pass `--repo`.
3. Do not create a PR from an unpushed local branch.
4. Check command output after each mutating action before continuing.
5. Prefer `--output json` when follow-up logic depends on IDs, URLs, or mergeability.
6. Use `tea api` as a fallback, not as the default path for standard issue and PR workflows.
7. Keep branch and base selection explicit for PR creation when there is any ambiguity.

## Recovery

- If `tea` acts on the wrong repo, rerun with `--repo owner/repo`.
- If `tea` picks the wrong account, rerun with `--login <login-name>`.
- If a PR command fails because the branch is unknown remotely, push the branch first and retry.
- If a high-level command cannot express the needed update, switch to `tea api`.
