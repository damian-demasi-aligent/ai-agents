# ai-agents
A collection of AI agents, skills, commands, and more.

## Agent Guide Switching

This repo supports multiple agent guides. The helper script copies the selected guide to the target project root and lets you choose between `AGENTS.md` (Cursor/Codex) or `CLAUDE.md` (Claude Code).

## Install Location and Setup

Clone this repo inside a `.cursor`, `.calude`, or `.codex` folder. After cloning, run the setup script to copy the `ai-agents` contents one level up and then run the guide selector.

- `./scripts/setup-agents magento2`
- `./scripts/setup-agents nextjs`

Structure:

- `agents/magento2.md`
- `agents/nextjs.md`
- `AGENTS.md` (copied to the target project root when selected)
- `CLAUDE.md` (copied to the target project root when selected)

### Switch Guides

Use the helper script to copy the guide. You will be prompted to choose `AGENTS.md` or `CLAUDE.md`:

- `./scripts/use-agent-guide magento2`
- `./scripts/use-agent-guide nextjs`

Optional target root (when this repo is nested inside another project):

- `./scripts/use-agent-guide magento2 /path/to/project-root`
- `./scripts/use-agent-guide nextjs /path/to/project-root`

If you see a permission error, run `chmod +x scripts/use-agent-guide` once.
