---
title: Tooling
summary: "Documents the tooling stack layered on top of the static HTML page: GitNexus, SonarQube, gitleaks, and OpenWiki."
topics: [architecture, tooling]
sources:
  - id: agents-md
    type: file
    path: AGENTS.md
  - id: sonar-project
    type: file
    path: sonar-project.properties
  - id: pre-commit-config
    type: file
    path: .pre-commit-config.yaml
  - id: gitnexus-meta
    type: file
    path: .gitnexus/gitnexus.json
  - id: gitnexus-run
    type: file
    path: .gitnexus/run.cjs
  - id: openwiki-quickstart
    type: file
    path: openwiki/quickstart.md
  - id: openwiki-last-update
    type: file
    path: openwiki/.last-update.json
---

# Tooling

The repository layers several tooling integrations on top of the static HTML page. These tools provide code intelligence, static analysis, secret scanning, and documentation generation without requiring a build step or framework.

## GitNexus

GitNexus provides code intelligence for the repository. It indexes the codebase into a knowledge graph, enabling symbol-level queries, impact analysis, and dependency tracing.

### Index State

The index stores the repository as a knowledge graph of symbols (nodes) and their relationships (edges), plus zero execution flows. Its node and edge counts grow as files are added or changed, so they drift over time and should not be read as fixed values. The index is stored in `.gitnexus/` and refreshed by running `node .gitnexus/run.cjs analyze --index-only` from the repository root[@gitnexus-meta].

### Runner Script

The `.gitnexus/run.cjs` file is a self-contained CJS script that resolves the best available command-line runner at execution time[@gitnexus-run]. It walks a version-gated ladder:

1. A globally installed `gitnexus` on PATH (best — no install step)
2. npm 11+ with `pnpm` on PATH → `pnpm --allow-build=… dlx` (avoids the npx arborist crash and the pnpm 10+ ignored-build-script failure)
3. npm 11+ without `pnpm` but with `bunx` → `bunx`
4. npm < 11 with `npm` on PATH → `npx`
5. `pnpm` only → `pnpm --allow-build=… dlx`
6. a bun-only machine (npm, npx, pnpm all absent) → `bunx`
7. last resort → `npx`

The ladder is version-gated because a Node toolchain is no longer assumed: on a bun-only machine every Node-based rung resolves to `npx`, which cannot run, so the two `bunx` rungs were added. Each `bunx` rung gates on `bunx` actually running, not merely existing on PATH. The `--allow-build` flags must precede the `dlx` token, because pnpm < 10.14 parses flags placed after `dlx` as package specs. PATH detection is spawn-free.

The script is regenerated on every `gitnexus analyze` run and is gitignored.

### Integration

AGENTS.md documents the GitNexus integration, specifying when to run impact analysis before edits, how to detect changes before commits, and which MCP resources to query for codebase context[@agents-md].

## SonarQube

Static analysis is configured via `sonar-project.properties`[@sonar-project]:

- **Project key:** `measurecamp_madrid_2025`
- **Project name:** `jrodeiro5/measurecamp_madrid_2025`
- **Source directory:** `.` (current directory)
- **Exclusions:** `node_modules/`, `__pycache__/`, `*.pyc`, `.venv/`, `venv/`, `dist/`, `build/`, `vendor/`, `openwiki/`

The `openwiki/` directory is excluded from analysis, as it contains generated documentation.

## Pre-commit Hook

A gitleaks pre-commit hook scans staged changes for secrets before each commit. It is configured in `.pre-commit-config.yaml` and runs `gitleaks protect --staged --verbose` on the system[@pre-commit-config].

## OpenWiki

 OpenWiki generates documentation from the repository. The `openwiki/quickstart.md` file documents the static HTML page[@openwiki-quickstart]. The `openwiki/.last-update.json` file records OpenWiki update metadata (model, command, gitHead, status) on every regeneration[@openwiki-last-update]; its values change each run, so the exact model string is not pinned.

No scheduled CI workflow exists in this repository. The `.github/workflows/` directory is empty.

## Related Pages

- For understanding the HTML page these tools operate on, see [Page Structure](../architecture/page-structure).
- For the decision to ship that page as a single static HTML file, see [Static HTML Choice](../decisions/static-html-choice).
