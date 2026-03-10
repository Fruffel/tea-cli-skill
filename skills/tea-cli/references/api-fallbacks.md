# API Fallbacks

Use this reference when `tea` subcommands do not expose the exact field or endpoint you need.

## Rules

- Run these commands from the target repository when possible so `{owner}` and `{repo}` resolve automatically.
- Add `--repo owner/repo` or `--login <login-name>` when repo context is not available.
- Prefer `-f` for plain strings and `-F` for typed fields.

## Read Pull Request Details

```bash
tea api repos/{owner}/{repo}/pulls/4
```

Use this when you need raw PR fields beyond what `tea pulls 4` prints by default.

## Create an Issue

```bash
tea api -X POST repos/{owner}/{repo}/issues \
  -f title='Bug: session leak' \
  -f body='Steps to reproduce and expected behavior'
```

Use this when `tea issues create` is not enough for the workflow or payload.

## Create a Pull Request

```bash
tea api -X POST repos/{owner}/{repo}/pulls \
  -f head='feature/branch' \
  -f base='main' \
  -f title='fix: handle login fallback' \
  -f body='Closes #12'
```

Use this when PR creation needs raw API control.

## Update an Issue or Pull Request

```bash
tea api -X PATCH repos/{owner}/{repo}/issues/12 \
  -f title='Updated title' \
  -f body='Updated body'
```

Gitea exposes pull requests through the issues API for several update operations, so this pattern is useful for both issues and PR metadata changes.

## Add a Review Comment or Other Unsupported Write

Start with the repository-scoped REST path you need and keep payloads explicit:

```bash
tea api -X POST repos/{owner}/{repo}/issues/12/comments \
  -f body='Please add a regression test for this path.'
```

If the high-level `tea` command exists and is sufficient, prefer that instead.
