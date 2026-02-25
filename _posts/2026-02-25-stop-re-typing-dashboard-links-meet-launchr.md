---
layout: post
title: "Stop Re-Typing Dashboard Links: Meet launchr"
description: "launchr helps teams open dashboards, logs, and runbook links from reusable command templates. Keep links typed, validated, and fast to execute."
category: dev-tools
tags: cli tips productivity
---

# Stop Re-Typing Dashboard Links: Meet launchr

Most teams still open dashboards the hard way: copy a link, tweak query params, repeat.

`launchr` is a CLI that removes that friction. You define command templates once, pass typed flags at runtime, and it opens the final URL in your browser.

It is fast, predictable, and built for repeatable operational workflows.

## What launchr is

`launchr` is a config-driven Node.js CLI (`Node.js 20+`) published as `launchr-cli`.

You define commands in a JSON config file, then run them like this:

```bash
launchr grafana -e production -q error -t 5m
```

Instead of manually assembling URLs, you type short flags and let `launchr` handle URL composition and validation.

## Why it is useful

Most link sharing in engineering teams is informal and inconsistent. `launchr` gives that process structure:

- Command definitions are reusable
- Parameters are typed (`string`, `integer`, `boolean`, `single-choice-list`)
- Required values are enforced
- Allowed values can be constrained
- Unknown flags and invalid inputs fail early

That means fewer malformed links, fewer repeated steps, and less context-switching during incidents or debugging sessions.

## Quick start

Install:

```bash
npm install -g launchr-cli
```

Create a command interactively:

```bash
launchr add
```

List configured commands:

```bash
launchr list
```

Run a custom command:

```bash
launchr <custom-command> [flags]
```

Show help:

```bash
launchr help
launchr <custom-command> help
```

## How configuration works

`launchr` stores command definitions in:

```text
~/.launchr-configurations/launchr-commands.json
```

Example command definition:

```json
{
  "grafana": {
    "description": "Open Grafana with typed parameters",
    "url": "https://grafana.com/{environment}/{query}/{timeframe}",
    "parameters": {
      "environment": {
        "type": "single-choice-list",
        "flag": "e",
        "defaultValue": "staging",
        "required": true,
        "values": ["staging", "production"]
      },
      "query": {
        "type": "string",
        "flag": "q",
        "defaultValue": "error",
        "required": true
      },
      "timeframe": {
        "type": "single-choice-list",
        "flag": "t",
        "defaultValue": "5m",
        "required": true,
        "values": ["5m", "10m", "1h", "6h"]
      }
    }
  }
}
```

This pattern is simple but powerful: once your team codifies recurring URL shapes, launching the right page becomes one command.

## Built for safe, repeatable CLI workflows

`launchr` includes strong validation across:

- Schema correctness
- URL placeholders mapped to parameter keys
- Missing required values
- Type mismatches
- Invalid enum-style choices

It also supports bootstrap behavior when config is missing, so first-time setup is straightforward.

## Current command naming (v1.1.0)

The canonical command for interactive setup is now:

```bash
launchr add
```

`launchr init` is still available in v1.x as a deprecated alias and is planned for removal in v2.0.0. If you are documenting team workflows, standardize on `launchr add` now.

## Final thoughts

`launchr` is a focused tool: it does not try to be a full automation platform. It solves one high-frequency problem well, turning repeated URL workflows into typed, reusable commands.

For teams that live in dashboards and operational tooling, that small shift can save real time every day.
