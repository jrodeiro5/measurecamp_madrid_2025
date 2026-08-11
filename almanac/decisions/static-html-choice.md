---
title: Static HTML Choice
summary: Records the decision to use a single static HTML file with no build step, no framework, and no package manager.
topics: [decisions, static-html]
sources:
  - id: index-html
    type: file
    path: index.html
  - id: agents-md
    type: file
    path: AGENTS.md
---

# Static HTML Choice

This repository uses a single static HTML file (`index.html`) with no build step, no framework, and no package manager. The choice was made to prioritize simplicity, zero deployment complexity, and direct editability.

## Context

The repository serves as a resource hub for a conference talk. The primary requirements were:

- **Simplicity** — The page should be easy to edit, understand, and maintain.
- **Zero deployment complexity** — No build step, no framework, no package manager.
- **Direct editability** — Anyone should be able to open the file and make changes.
- **Conference context** — The page is temporary, tied to a single event.

## Decision

The repository contains a single `index.html` file. All styling is embedded in `<style>` tags. No JavaScript, no bundler, no package.json, no build configuration.

External resources (Google Fonts, images) are loaded via CDN or direct URLs. The page is deployed as a static site.

The decision is reflected in `index.html`[@index-html], which contains all styling inline and no external dependencies beyond CDN-loaded fonts and images.

## Consequences

### Positive

- **Direct editability** — Anyone can open the file in any text editor and make changes.
- **Zero deployment complexity** — No build step, no framework, no package manager.
- **Fast to understand** — The entire repository is one file.
- **Low maintenance** — No dependencies to update, no build failures to debug.

### Negative

- **No build-time optimizations** — No minification, no tree-shaking, no code splitting.
- **No framework features** — No component model, no state management, no routing.
- **Limited reusability** — The page is tied to a specific event and author.

### Neutral

- **Single file** — The repository contains only `index.html` as source. All other files are configuration or tooling metadata.
- **External dependencies** — The page relies on Google Fonts and external images, which could become unavailable.

The AGENTS.md file[@agents-md] documents the tooling integrations layered on this choice: GitNexus, SonarQube, gitleaks, and OpenWiki.

## Related Pages

- For details on the page structure, see [Page Structure](../architecture/page-structure).
- For understanding the tooling layered on this choice, see [Tooling](../architecture/tooling).
