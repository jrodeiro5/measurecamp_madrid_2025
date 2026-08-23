---
title: Getting Started
summary: The front door to the wiki - explains what this repository is and where to start reading.
topics: [navigation]
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
---

# Getting Started

This repository is a single static HTML page (`index.html`[@index-html]) used as a link-in-bio resource hub for Javier Rodeiro's talk at MeasureCamp Madrid 2025. The talk topic is Model Context Protocol (MCP). The page links to personal profiles, projects, and presentation resources.

The repository contains one source file plus configuration and tooling metadata: a GitNexus code intelligence index (documented in AGENTS.md[@agents-md]), a SonarQube configuration (`sonar-project.properties`[@sonar-project]), a gitleaks pre-commit hook (`.pre-commit-config.yaml`[@pre-commit-config]), and OpenWiki-generated documentation. There is no build step, no framework, and no package manager. Detailed tooling descriptions live on the [Tooling](architecture/tooling) page.

## What to Read

- For the HTML page structure, CSS design system, and linked resources, read [Page Structure](architecture/page-structure).
- For the tooling stack layered on the static page, read [Tooling](architecture/tooling).
- For what Model Context Protocol (MCP) is and why this repository exists, read [Model Context Protocol](concepts/model-context-protocol).
- For the choice to ship a single static HTML file, read [Static HTML Choice](decisions/static-html-choice).

## Related Pages

- [Page Structure](architecture/page-structure)
- [Tooling](architecture/tooling)
- [Model Context Protocol](concepts/model-context-protocol)
- [Static HTML Choice](decisions/static-html-choice)

This repository is hosted as a static site and branded with Ayesa and MeasureCamp Madrid visuals.
