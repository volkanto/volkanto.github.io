---
layout: post
title: "Stop Hand-Building GitHub Releases: Meet Yaba"
description: "yaba-release-cli turns that into a repeatable CLI workflow with explicit commands, config-driven defaults, and machine-readable output for CI."
category: dev-tools
tags: cli tips productivity
---

Most teams still run release day with manual glue: compare tags, paste changelogs, choose a tag format, create the release, then post to Slack.

`yaba-release-cli` turns that into a repeatable CLI workflow with explicit commands, config-driven defaults, and machine-readable output for CI.

It runs on Node.js, but it can create releases for **any GitHub-hosted project** (Go, Python, Java, Rust, JavaScript, monorepos, and more).

## What Yaba is

`yaba-release-cli` is a GitHub release operations CLI (`Node.js >= 22`) with a clear command model:

- `yaba release preview`
- `yaba release create`
- `yaba doctor`
- `yaba config init`
- `yaba config validate`

You can run it interactively for local workflows, or non-interactively in CI with `--no-prompt` and `--format json`.

## Why it is useful

Yaba replaces ad-hoc release scripts with a stable contract:

- Preview before mutation (`release preview`)
- Deterministic exit codes for automation (`0..6`)
- Config precedence across flags/env/project/user/defaults
- Release policy controls (`--allow-empty`, `--fail-on-empty`, `--max-commits`)
- Strategy-based tag generation (`pattern`, `semver`, `sha`)
- Slack publishing with retry/backoff on transient failures
- Auth/network/repo diagnostics through `doctor`

That means less release-day guesswork and more predictable CI behavior.

## Quick start

Install:

```bash
npm install -g yaba-release-cli
```

Set credentials:

```bash
export YABA_GITHUB_ACCESS_TOKEN=your_token
# optional
export YABA_GITHUB_REPO_OWNER=my-org
export YABA_SLACK_HOOK_URL=https://hooks.slack.com/services/...
```

Run diagnostics:

```bash
yaba doctor
```

Initialize and validate config:

```bash
yaba config init
yaba config validate --format json
```

Preview release payload:

```bash
yaba release preview --repo my-repo --target main --format json
```

Create the release:

```bash
yaba release create --repo my-repo --tag-strategy semver --no-prompt --publish
```

## How configuration works

Yaba resolves runtime values using this precedence:

```text
flags > env vars > ./yaba.config.json > ~/.config/yaba/config.json > defaults
```

Example `yaba.config.json`:

```json
{
  "github": { "owner": "my-org" },
  "release": {
    "repo": "my-repo",
    "tagStrategy": "semver",
    "tagOnConflict": "increment",
    "tagMaxAttempts": 20,
    "target": "main",
    "failOnEmpty": true,
    "maxCommits": 200,
    "interactive": false
  },
  "notifications": {
    "providers": ["slack"],
    "slack": { "enabled": true }
  },
  "output": { "format": "json" }
}
```

## Built for safe, repeatable CLI workflows

Yaba includes strict operational checks:

- Validates configuration schema (`config validate`)
- Rejects conflicting safety flags early
- Supports first-release fallback comparison behavior
- Generates GitHub release notes with PR-label grouping when metadata exists
- Falls back to commit-list rendering when metadata is missing
- Keeps JSON output stable for machine consumers

For notifications, Slack publishing retries transient failures (`429`, `408`, `5xx`, network codes) with exponential backoff (default: 3 attempts).

## Current command naming (v2.x)

Use explicit command form in scripts:

```bash
yaba release create
```

Legacy forms are still accepted in v2 but deprecated:

- `--yes` -> use `--no-prompt`
- `--notify slack` -> use `--publish`
- `--release-name` -> use `--name`
- implicit `yaba` -> use `yaba release create`

Standardizing now avoids migration friction in the next major version.

## Final thoughts

Yaba is focused tooling. It does not try to replace CI/CD platforms; it standardizes one high-frequency surface area: GitHub release operations.

If your team wants release creation to be deterministic, scriptable, and language-agnostic at the repository level, Yaba is a practical fit.
