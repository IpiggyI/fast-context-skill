# Fast Context Skill

Fast Context Skill is a Codex Skill and CLI adaptation of [`SammySnake-d/fast-context-mcp`](https://github.com/SammySnake-d/fast-context-mcp).

It is a fork-style rewrite of the original MCP project: the Windsurf Devstral semantic search core is vendored from upstream `v1.3.0-beta.2`, while the MCP server wrapper is removed. Codex can use this as a Skill by running a local script, which avoids loading the Fast Context MCP tool schema in every session.

## What Changed From The MCP Repo

- Keeps the upstream semantic search loop that talks to Windsurf Devstral.
- Keeps local command execution helpers for `rg`, file reads, tree output, and context gathering.
- Removes the MCP server entry point from the runtime path.
- Adds a Codex `SKILL.md` workflow.
- Adds `scripts/fast-context-search.mjs` as the direct CLI entry point.
- Adds npm packaging and GitHub Actions publishing.

## Install As A Codex Skill

```bash
git clone https://github.com/oulkurt/fast-context-skill.git ~/.codex/skills/fast-context
cd ~/.codex/skills/fast-context
npm install
```

Then use `$fast-context` in Codex. In a fresh Codex session, the Skill will instruct Codex to run:

```bash
node ~/.codex/skills/fast-context/scripts/fast-context-search.mjs \
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

The script can usually discover the key automatically from a logged-in Windsurf desktop installation:

```bash
node scripts/fast-context-search.mjs --check-key
```

That command prints only a masked key and the local database path. It does not print the full secret.

If you need to set the key explicitly for a shell session:

```bash
export WINDSURF_API_KEY="<your-windsurf-api-key>"
```

If you need to extract the full key locally, run this from the repository root:

```bash
node --input-type=module -e 'import { extractKey } from "./scripts/lib/extract-key.mjs"; const r = await extractKey(); if (r.error) { console.error(r.error); process.exit(1); } console.log(r.api_key);'
```

Treat the value like any other API secret:

- Do not commit it.
- Do not paste it into GitHub issues, README files, workflows, or logs.
- Do not add it as an npm publish secret. npm publishing uses `NPM_TOKEN`, not `WINDSURF_API_KEY`.

## npm Publishing

The repository includes `.github/workflows/npm-publish.yml`.

The workflow runs on:

- manual `workflow_dispatch`
- pushed version tags such as `v0.1.0`
- a weekly scheduled check

On schedule, it checks whether the current package version already exists on npm. If it exists, the workflow skips publishing. If it does not exist, the workflow publishes.

To enable publishing:

1. Create an npm automation token.
2. Add it to the GitHub repository secrets as `NPM_TOKEN`.
3. Push a version tag or run the workflow manually.

Example release:

```bash
npm version patch
git push --follow-tags
```

## Development

```bash
npm install
npm test
npm pack --dry-run
```

## Attribution

This repository vendors and adapts code from [`SammySnake-d/fast-context-mcp`](https://github.com/SammySnake-d/fast-context-mcp), originally licensed under MIT. The vendored upstream license is preserved at `scripts/lib/LICENSE.fast-context-mcp`.

This project is not affiliated with Windsurf.
