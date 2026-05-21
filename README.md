# Fast Context Skill

[![skills.sh](https://skills.sh/b/oulkurt/fast-context-skill)](https://skills.sh/oulkurt/fast-context-skill)

Fast Context Skill is an Agent Skill and CLI adaptation of [`SammySnake-d/fast-context-mcp`](https://github.com/SammySnake-d/fast-context-mcp).

It is a fork-style rewrite of the original MCP project: the Windsurf Devstral semantic search core is vendored from upstream `v1.3.0-beta.2`, while the MCP server wrapper is removed. Any Skills-compatible agent that can run local scripts can use this Skill; Codex is one supported install target, not a requirement.

## What Changed From The MCP Repo

- Keeps the upstream semantic search loop that talks to Windsurf Devstral.
- Keeps local command execution helpers for `rg`, file reads, tree output, and context gathering.
- Removes the MCP server entry point from the runtime path.
- Adds an agent-friendly `SKILL.md` workflow.
- Adds `scripts/fast-context-search.mjs` as the direct CLI entry point.
- Adds npm packaging for direct CLI use.

## Install With skills CLI

One-command install via the open `skills` CLI:

```bash
npx skills add oulkurt/fast-context-skill --skill fast-context -y
```

To install to every agent target supported by your local `skills` CLI:

```bash
npx skills add oulkurt/fast-context-skill --skill fast-context -a '*' -y
```

Use the `-a` flag when you want to target a specific agent supported by your `skills` CLI version. For Codex:

```bash
npx skills add oulkurt/fast-context-skill --skill fast-context -a codex -y
```

For a global Codex install specifically:

```bash
npx skills add oulkurt/fast-context-skill --skill fast-context -a codex -g -y
```

Manual install is agent-specific: clone this repository into the skill directory your agent reads, then run `npm install` from that directory. For Codex:

```bash
git clone https://github.com/oulkurt/fast-context-skill.git ~/.codex/skills/fast-context
cd ~/.codex/skills/fast-context
npm install
```

Then invoke `$fast-context` or your agent's skill invocation convention. The Skill instructs the agent to run the bundled script from the installed skill directory, for example:

```bash
node /path/to/installed/fast-context/scripts/fast-context-search.mjs \
  --project "/absolute/path/to/project" \
  --query "Where is authentication implemented?"
```

## Use As A CLI

From a clone:

```bash
npm install
node scripts/fast-context-search.mjs \
  --project "/absolute/path/to/project" \
  --query "Where is the database connection pool configured?"
```

From npm after publish:

```bash
npx fast-context-skill \
  --project "/absolute/path/to/project" \
  --query "Where is the database connection pool configured?"
```

## Windsurf API Key

This project still depends on Windsurf's Devstral backend. It replaces the MCP layer, not the Windsurf backend.

The script can usually discover the key automatically from a logged-in Windsurf desktop installation, so most users do not need to copy a key manually.

After installing the Skill, verify local key discovery:

```bash
node /path/to/installed/fast-context/scripts/fast-context-search.mjs --check-key
```

For a project install, run this from the project root:

```bash
node .agents/skills/fast-context/scripts/fast-context-search.mjs --check-key
```

For Codex global installs, that path is usually:

```bash
node ~/.codex/skills/fast-context/scripts/fast-context-search.mjs --check-key
```

`--check-key` prints only a masked key and the local database path. It does not print the full secret.

If you need the full key locally, print it with:

```bash
node /path/to/installed/fast-context/scripts/fast-context-search.mjs --print-key
```

If you want to set `WINDSURF_API_KEY` for the current shell in one command:

```bash
eval "$(node /path/to/installed/fast-context/scripts/fast-context-search.mjs --key-env)"
```

For a project install, the same command is:

```bash
eval "$(node .agents/skills/fast-context/scripts/fast-context-search.mjs --key-env)"
```

From a repository clone:

```bash
node scripts/fast-context-search.mjs --check-key
node scripts/fast-context-search.mjs --print-key
eval "$(node scripts/fast-context-search.mjs --key-env)"
```

Without cloning, you can run the GitHub package directly with `npx`:

```bash
npx --yes github:oulkurt/fast-context-skill --check-key
eval "$(npx --yes github:oulkurt/fast-context-skill --key-env)"
```

Treat the value like any other API secret:

- Do not commit it.
- Do not paste it into GitHub issues, README files, workflows, or logs.

## Development

```bash
npm install
npm test
npm pack --dry-run
```

## Attribution

This repository vendors and adapts code from [`SammySnake-d/fast-context-mcp`](https://github.com/SammySnake-d/fast-context-mcp), originally licensed under MIT. The vendored upstream license is preserved at `scripts/lib/LICENSE.fast-context-mcp`.

This project is not affiliated with Windsurf.
