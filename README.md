# ai-agents
A collection of AI agents, skills, commands, and more.

## Agent Guide Switching

This repo supports multiple agent guides. The active guide is exposed at the repo root as `AGENTS.md`, and `CLAUDE.md` is a symlink to `AGENTS.md`.

Structure:

- `AGENTS/magento2.md`
- `AGENTS/nextjs.md`
- `AGENTS.md` (symlink to one of the above)
- `CLAUDE.md` (symlink to `AGENTS.md`)

### Switch Guides

Use the helper script to rewire the symlink:

- `./scripts/use-agent-guide magento2`
- `./scripts/use-agent-guide nextjs`

If you see a permission error, run `chmod +x scripts/use-agent-guide` once.
