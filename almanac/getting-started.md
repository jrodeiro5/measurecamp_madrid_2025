---
title: Getting Started
summary: The front door to the wiki — explains what this repository is, what it contains, and where to start reading.
topics: [concepts]
sources:
  - id: index-html
    type: file
    path: index.html
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
  - id: openwiki-quickstart
    type: file
    path: openwiki/quickstart.md
---

# Getting Started

This repository is a single static HTML page used as a link-in-bio resource hub for Javier Rodeiro's talk at MeasureCamp Madrid 2025. The talk topic is Model Context Protocol (MCP). The page links to personal profiles, projects, and presentation resources.

The repository contains one source file (`index.html`[@index-html]), a GitNexus code intelligence index (see AGENTS.md[@agents-md]), a SonarQube configuration, a gitleaks pre-commit hook, and OpenWiki documentation. There is no build step, no framework, and no package manager.

## What to Read

Start with the page on the system you are working with:

- For understanding the HTML page structure, CSS design, and linked resources, read [Page Structure](architecture/page-structure).
- For understanding the tooling stack layered on top of the static page, read [Tooling](architecture/tooling).
- For understanding what Model Context Protocol (MCP) is and why this repository exists, read [Model Context Protocol](concepts/model-context-protocol).

## Repository Contents

| File | Purpose |
|------|---------|
| `index.html` | The only source file — a styled link directory |
| `AGENTS.md` | GitNexus and OpenWiki integration documentation |
| `sonar-project.properties` | SonarQube static analysis config |
| `.pre-commit-config.yaml` | gitleaks pre-commit hook |
| `.gitnexus/` | GitNexus code intelligence index |
| `openwiki/` | OpenWiki generated documentation |

The repository is hosted as a static site and branded with Ayesa and MeasureCamp Madrid visuals.

## Tooling Details

### GitNexus

The repository is indexed by GitNexus for code intelligence. The index state shows 14 symbols, 10 relationships, and 0 execution flows[@gitnexus-meta]. The index is refreshed by running `node .gitnexus/run.cjs analyze` from the repository root.

### SonarQube

Static analysis is configured via `sonar-project.properties`[@sonar-project]. The project key is `measurecamp_madrid_2025` and the source directory is `.` (current directory). The `openwiki/` directory is excluded from analysis.

### Pre-commit Hook

A gitleaks pre-commit hook scans staged changes for secrets before each commit. It is configured in `.pre-commit-config.yaml` and runs `gitleaks protect --staged --verbose` on the system[@pre-commit-config].

### OpenWiki

OpenWiki generates documentation from the repository. The `openwiki/quickstart.md` file documents the static HTML page[@openwiki-quickstart]. The `openwiki/.last-update.json` file contains update metadata (model: `mimo-v2.5-pro`, command: `init`).

No scheduled CI workflow exists in this repository — the `.github/workflows/` directory is empty.

## Related Pages

- For details on how this page is structured and implemented, see [Page Structure](architecture/page-structure).
- For understanding the tooling layered on this page, see [Tooling](architecture/tooling).
- For understanding why this page exists, see [Model Context Protocol](concepts/model-context-protocol).
