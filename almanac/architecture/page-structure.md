---
title: Page Structure
summary: Documents the HTML page structure, CSS design system, and linked resources of the static site.
topics: [architecture]
sources:
  - id: index-html
    type: file
    path: index.html
---

# Page Structure

The repository contains a single static HTML file (`index.html`) that renders a styled link directory. The page uses no JavaScript, no build step, no framework, and no package manager. This shape is the result of the [Static HTML Choice](../decisions/static-html-choice), documented there with its context and consequences. It is designed to be directly editable and deployable as a static site.

## Layout

The page uses a centered container with maximum width of 1200px and responsive padding. Content is organized into a `<header>`, three `.section` cards, and a `<footer>`:

1. **Header** — Centered logo and title (inline `<header>`, not a card)
2. **Personal Links** — Grid of link cards
3. **Projects** — Grid of project links
4. **Presentation Resources** — Grid of MCP resource links
5. **Footer** — Ayesa logo and copyright (inline `<footer>`, not a card)

The three content sections use the `.section` class with rounded corners and semi-transparent white background. The header and footer use native HTML elements without the card styling.

## CSS Design System

### Custom Properties

The page defines a small set of CSS custom properties for theming:

| Variable | Value | Usage |
|----------|-------|-------|
| `--ayesa-blue` | `#005C96` | Gradient anchor |
| `--ayesa-dark-blue` | `#003A5D` | Gradient start, link card text |
| `--measurecamp-orange` | `#FF6B35` | Gradient end |
| `--measurecamp-yellow` | `#FFD23F` | Section headings |
| `--white` | `#FFFFFF` | Body text |

### Background

A 135-degree linear gradient runs from `--ayesa-dark-blue` to `--measurecamp-orange`, covering the full viewport height.

### Typography

The page loads the Montserrat typeface (weights 300, 400, 500, 600, 700) from Google Fonts. The fallback stack is `Montserrat, 'Segoe UI', Arial, sans-serif`.

### Animations

Two CSS keyframe animations control entry effects:

- `fadeIn` — Fades elements from opacity 0 to 1 over 1 second
- `slideUp` — Slides elements up from translateY(20px) to 0 with staggered delays

The header fades in, while each section slides up with incremental delays (0.2s, 0.4s, 0.6s).

### Responsive Layout

The link grid uses CSS Grid with `auto-fit` and `minmax(250px, 1fr)` columns. A 768px media query reduces container padding and heading font size for mobile.

## External Resources

The page references external images:

- MeasureCamp Madrid logo from `spain.measurecamp.org`
- Ayesa logo from `ayesa.com`

Both are loaded via `<img>` tags with `alt` attributes for accessibility.

The CSS design is defined in `index.html`[@index-html], using custom properties for theming and CSS Grid for responsive layout.

## Related Pages

- For understanding the tooling layered on this page, see [Tooling](tooling).
- For the choice to ship a single static HTML file, see [Static HTML Choice](../decisions/static-html-choice).
- For understanding why this page exists, see [Model Context Protocol](../concepts/model-context-protocol).
