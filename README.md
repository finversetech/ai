# Finverse AI Skills

Cursor Agent Skills for Finverse integrations. These skills teach AI assistants how to implement Finverse payment flows, APIs, and best practices.

## Overview

This repository contains **project skills** that Cursor's AI agent can use when helping you build Finverse integrations. Skills provide domain-specific instructions, API patterns, and implementation checklists so the agent can generate accurate, production-ready code.

## Available Skills

| Skill | Description |
|-------|-------------|
| [finverse-online-payment](skills/finverse-online-payment/SKILL.md) | Implements Finverse online payment flow: create payment link, redirect, callback, and status polling. Use when integrating Finverse payments, building checkout pages, or working with payment links. |

## Installation

### Option A: Cursor Agent (easiest)

Copy the GitHub repository URL, open Cursor Agent chat, and type:

> Install the agent skills from this repository into my current project: https://github.com/finversetech/ai

The agent will clone the repo and copy the skills into your project's `.cursor/skills/` folder.

### Option B: Command line

Install the agent skills from this repository into your current project:

```bash
mkdir -p .cursor && git clone --depth 1 https://github.com/finversetech/ai.git .cursor/finverse-ai-src && cp -r .cursor/finverse-ai-src/skills .cursor/ && rm -rf .cursor/finverse-ai-src
```

## Using These Skills

Once installed, Cursor automatically applies these skills when your prompts match their scope. You can also invoke a skill manually with its slash command (e.g. `/finverse-online-payment`).

For example:

- **finverse-online-payment** — Ask the agent to "implement Finverse checkout", "add payment links to my app", or "integrate Finverse payments". Or type `/finverse-online-payment` to invoke it directly. The agent will follow the flow (create link → redirect → callback → poll status) and generate code for your tech stack.

Skills live in `.cursor/skills/` and are available only in that project. To use them across all projects, copy individual skills to `~/.cursor/skills/` instead.

## Resources

- [Finverse Dashboard](https://dashboard.finverse.com)
- [Finverse Payment API Docs](https://docs.finverse.com/#4c635345-e445-4d7b-bd4a-92506e98ada3)
- [Finverse Swagger](https://api.prod.finverse.net/swagger.json)
- [Finverse Go SDK](https://github.com/finversetech/sdk-go)
- [Finverse TypeScript SDK](https://github.com/finversetech/sdk-typescript)

## License

See repository license.
