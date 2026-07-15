# Quarto Academic & Personal Website — Design Spec

**Date:** 2026-07-15
**Owner:** Maëlle Delouis
**Status:** Approved (design), pending implementation plan

## Purpose

A minimal, academic personal website for Maëlle Delouis, PhD student in the
Department of Sociology (with a political-science lean) at the University of
Zurich. Built with Quarto, deployed to GitHub Pages. Content is placeholder
and will be filled in by the owner later; the deliverable is a complete,
working, editable site scaffold.

## Owner profile (for content framing)

- PhD student, Department of Sociology, University of Zurich; research interests
  close to political science.
- Content (publications, bio, CV entries) will be added later — use clearly
  marked placeholders throughout.

## Decisions (locked)

| Area | Decision |
|------|----------|
| Sections | Home / About, Research, CV (no blog, no teaching page) |
| Aesthetic | Minimal & academic — whitespace, restrained type, single accent |
| Color mode | Light default, working dark-mode toggle |
| Publications | Manual Markdown list (no BibTeX automation) |
| Hosting | GitHub Pages via GitHub Actions on push to `main` |
| Domain | Host default URL for now (custom domain deferred, documented) |
| Base theme | Quarto `litera` + custom SCSS overrides |
| Fonts | Serif headings (Lora or Source Serif 4), sans-serif body |
| Accent | Muted slate/UZH-ish blue, defined as a single SCSS variable |

## Structure

```
Website/
├── _quarto.yml              # site config: title, navbar, theme, output
├── index.qmd                # Home / About (photo, bio, links)
├── research.qmd             # Research (manual Markdown publication list)
├── cv.qmd                   # CV page + link to PDF
├── styles.scss              # light theme: accent color, serif headings, spacing
├── styles-dark.scss         # dark theme overrides
├── assets/
│   ├── profile.jpg          # placeholder headshot
│   └── delouis-cv.pdf       # placeholder downloadable CV
├── .github/workflows/publish.yml   # auto-deploy to GitHub Pages
├── .gitignore
└── README.md                # edit / run-locally / deploy instructions
```

## Pages

### Home / About (`index.qmd`)
- Headshot (placeholder `assets/profile.jpg`).
- Name + role line: "PhD Student, Department of Sociology, University of Zurich".
- 2–3 placeholder bio paragraphs.
- Row of icon links: email, Google Scholar, ORCID, GitHub, Bluesky, LinkedIn,
  and a "Download CV" button. Owner deletes any not used. All hrefs are
  placeholders.

### Research (`research.qmd`)
- Plain Markdown, editable as text.
- Placeholder sections: **Peer-reviewed articles**, **Working papers**,
  **Work in progress** — each with 2–3 dummy entries formatted as
  author(s), year, title, venue, with placeholder links (PDF / DOI).

### CV (`cv.qmd`)
- Prominent "Download PDF" button → `assets/delouis-cv.pdf` (placeholder).
- Structured placeholder sections: Education, Positions, Publications,
  Teaching, Awards.

## Navigation & chrome

- Top navbar: **Home · Research · CV** (left); social icons + dark-mode toggle
  (right).
- Footer: copyright + "Built with Quarto".

## Theming details

- `_quarto.yml` uses `theme:` with `light: [litera, styles.scss]` and
  `dark: [litera, styles-dark.scss]` (or a suitable dark base) so the navbar
  toggle works.
- `styles.scss` defines the accent as a single Sass variable near the top
  (e.g. `$accent`) so the owner can recolor the whole site in one line.
- Serif headings + sans body via Quarto/Bootstrap font Sass variables; fonts
  pulled from Google Fonts.
- Generous spacing, understated links, no heavy shadows or cards (minimal
  academic).

## Deployment

- `.github/workflows/publish.yml`: on push to `main`, uses
  `quarto-actions/setup` + `quarto-actions/publish` (or render + peaceiris
  pages deploy) to publish to a `gh-pages` branch. No R execution needed
  (content is static Markdown), so CI stays lightweight.
- README documents the one-time setup: create GitHub repo, enable Pages
  (source = `gh-pages` branch), push to `main`. Also documents local preview
  (`quarto preview`) and how to add a custom domain later (CNAME).

## Explicitly out of scope

- Blog / listing pages.
- Teaching page.
- BibTeX / citation automation.
- Custom domain setup (documented as a later step only).

## Success criteria

- `quarto render` (or `quarto preview`) builds the three-page site with no
  errors.
- Site is visually minimal-academic: serif headings, single accent, working
  light/dark toggle.
- All content is placeholder but structurally complete and obvious to edit.
- Pushing to `main` on a configured GitHub repo publishes the live site.
