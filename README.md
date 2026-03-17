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
cd /path/to/your/project
mkdir -p .cursor
git clone --depth 1 https://github.com/finversetech/ai.git .cursor/finverse-ai-src
cp -r .cursor/finverse-ai-src/skills .cursor/
rm -rf .cursor/finverse-ai-src
```

Or as a one-liner (run from your project root):

```bash
mkdir -p .cursor && git clone --depth 1 https://github.com/finversetech/ai.git .cursor/finverse-ai-src && cp -r .cursor/finverse-ai-src/skills .cursor/ && rm -rf .cursor/finverse-ai-src
```

## Using These Skills

### Option 1: Project Skills (Recommended)

Copy or symlink the `skills/` directory into your project's `.cursor/skills/` folder:

```bash
mkdir -p .cursor
cp -r skills .cursor/
```

Skills in `.cursor/skills/` are automatically available when working in that project.

### Option 2: Personal Skills

Copy skills to your personal skills directory for use across all projects:

```bash
cp -r skills/finverse-online-payment ~/.cursor/skills/
```

## Project Structure

```
ai/
├── README.md
└── skills/
    └── finverse-online-payment/
        ├── SKILL.md              # Main skill instructions
        └── references/           # API specs, sequence diagrams
            ├── create-payment-mode-payment-link.md
            ├── create-setup-mode-payment-link.md
            ├── online-payment-flow.wsd
            └── payment-method-setup-flow.wsd
```

## Adding New Skills

1. Create a new directory under `skills/` (e.g. `skills/your-skill-name/`)
2. Add a `SKILL.md` with YAML frontmatter and instructions
3. Include optional `references/` for detailed docs
4. Follow the [Cursor Skill authoring guide](https://docs.cursor.com/context/rules-for-ai#skills) for structure and best practices

## Resources

- [Finverse Dashboard](https://dashboard.finverse.com)
- [Finverse Payment API Docs](https://docs.finverse.com/#4c635345-e445-4d7b-bd4a-92506e98ada3)
- [Finverse Swagger](https://api.prod.finverse.net/swagger.json)
- [Finverse Go SDK](https://github.com/finversetech/sdk-go)
- [Finverse TypeScript SDK](https://github.com/finversetech/sdk-typescript)

## License

See repository license.
