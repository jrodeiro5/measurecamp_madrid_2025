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

The `.gitnexus/run.cjs` file is a self-contained CJS script that resolves the best available command-line runner at execution time[@gitnexus-run]. It checks for:

1. A globally installed `gitnexus` on PATH
2. `pnpm` with `dlx` (with `--allow-build` flags for native dependencies)
3. `npx` as a fallback

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

OpenWiki generates documentation from the repository. The `openwiki/quickstart.md` file documents the static HTML page[@openwiki-quickstart]. The `openwiki/.last-update.json` file contains update metadata (model: `mimo-v2.5-pro`, command: `init`)[@openwiki-last-update].

No scheduled CI workflow exists in this repository. The `.github/workflows/` directory is empty.

## Related Pages

- For understanding the HTML page these tools operate on, see [Page Structure](../architecture/page-structure).
- For the decision to ship that page as a single static HTML file, see [Static HTML Choice](../decisions/static-html-choice).
