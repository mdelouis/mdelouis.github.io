# Quarto Academic & Personal Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build and deploy a minimal, academic three-page (Home/About, Research, CV) personal website for Maëlle Delouis using Quarto, published to GitHub Pages.

**Architecture:** A static Quarto website project. Content lives in three `.qmd` files rendered by Quarto's `website` project type. Visual style comes from the `litera` base theme plus two SCSS override files (light/dark). Deployment is a GitHub Actions workflow that renders and publishes to a `gh-pages` branch on push to `main`. No code execution in content, so CI needs only Quarto (no R/Python).

**Tech Stack:** Quarto 1.7.29, Bootstrap/SCSS theming, GitHub Actions, GitHub Pages.

## Global Constraints

- Quarto version: 1.7.29 (installed locally; CI pins via `quarto-actions/setup`).
- All user content is **placeholder** — clearly marked, structurally complete, obvious to edit. Never invent real publications, dates, or bio facts.
- Aesthetic: minimal & academic — serif headings, sans-serif body, generous whitespace, a single accent color defined as one SCSS variable, no heavy cards/shadows.
- Color mode: light default with a working dark-mode toggle in the navbar.
- Sections are exactly three: Home/About, Research, CV. No blog, no teaching page, no BibTeX automation, no custom domain (documented as a later step only).
- Publications on the Research page are hand-written Markdown, not generated from `.bib`.
- Site title / name: "Maëlle Delouis". Role line: "PhD Student, Department of Sociology, University of Zurich".
- The verification cycle for every task is `quarto render` (and/or `quarto preview`) succeeding with no errors, plus a visual check — there are no unit tests.

---

### Task 1: Initialize git repo and Quarto website skeleton

**Files:**
- Create: `.gitignore`
- Create: `_quarto.yml`
- Create: `index.qmd`, `research.qmd`, `cv.qmd` (minimal stubs)
- Create: `assets/.gitkeep`

**Interfaces:**
- Produces: a buildable Quarto `website` project with three pages and a navbar. Later tasks add real content and styling. Output dir is `_site` (Quarto default).

- [ ] **Step 1: Initialize git**

```bash
cd "/Users/maelledelouis/Library/CloudStorage/Dropbox-Personal/Website"
git init -b main
```

- [ ] **Step 2: Create `.gitignore`**

```gitignore
# Quarto
/_site/
/.quarto/
*_files/

# OS
.DS_Store
Thumbs.db
```

- [ ] **Step 3: Create `_quarto.yml`**

```yaml
project:
  type: website
  output-dir: _site

website:
  title: "Maëlle Delouis"
  navbar:
    left:
      - href: index.qmd
        text: Home
      - href: research.qmd
        text: Research
      - href: cv.qmd
        text: CV
    tools:
      - icon: envelope
        href: "mailto:maelle.delouis@uzh.ch"
        text: Email
  page-footer:
    center: "© 2026 Maëlle Delouis · Built with [Quarto](https://quarto.org)"

format:
  html:
    theme:
      light: [litera, styles.scss]
      dark: [litera, styles-dark.scss]
    toc: false
    css: []
```

Note: `styles.scss` / `styles-dark.scss` are created in Task 3. Until then, comment out the `theme:` block's scss entries OR create empty placeholder files. To keep this task self-contained and buildable, create empty placeholders now:

- [ ] **Step 4: Create empty SCSS placeholders so the build succeeds**

Create `styles.scss` with:

```scss
/*-- scss:defaults --*/
// styling added in Task 3
```

Create `styles-dark.scss` with:

```scss
/*-- scss:defaults --*/
// styling added in Task 3
```

- [ ] **Step 5: Create minimal page stubs**

`index.qmd`:

```markdown
---
title: "Maëlle Delouis"
---

Home page — content added in Task 4.
```

`research.qmd`:

```markdown
---
title: "Research"
---

Research page — content added in Task 5.
```

`cv.qmd`:

```markdown
---
title: "CV"
---

CV page — content added in Task 6.
```

- [ ] **Step 6: Create assets placeholder**

```bash
mkdir -p assets && touch assets/.gitkeep
```

- [ ] **Step 7: Render and verify build succeeds**

Run: `quarto render`
Expected: completes with no errors; `_site/index.html`, `_site/research.html`, `_site/cv.html` exist.

Verify: `ls _site/*.html`

- [ ] **Step 8: Commit**

```bash
git add -A
git commit -m "feat: scaffold Quarto website with three pages and navbar"
```

---

### Task 2: Add placeholder assets (headshot + CV PDF)

**Files:**
- Create: `assets/profile.jpg` (placeholder image)
- Create: `assets/delouis-cv.pdf` (placeholder PDF)

**Interfaces:**
- Produces: `assets/profile.jpg` (referenced by `index.qmd` in Task 4) and `assets/delouis-cv.pdf` (linked from `index.qmd` and `cv.qmd`).

- [ ] **Step 1: Create a placeholder headshot**

Generate a simple neutral placeholder square image so the layout is testable. Use ImageMagick if available:

```bash
magick -size 600x600 xc:'#e9edf2' -gravity center -pointsize 40 -fill '#7a8899' -annotate 0 'Photo' assets/profile.jpg 2>/dev/null || convert -size 600x600 xc:'#e9edf2' -gravity center -pointsize 40 -fill '#7a8899' -annotate 0 'Photo' assets/profile.jpg
```

If neither `magick` nor `convert` exists, create the placeholder with a tiny Python/PIL script; if PIL is unavailable, download a neutral placeholder is NOT allowed (offline-safe). Fallback: copy any 1x1 and note in README that the owner must replace it. Preferred path is ImageMagick — confirm it exists first with `which magick convert`.

- [ ] **Step 2: Create a placeholder CV PDF**

```bash
quarto pandoc -o assets/delouis-cv.pdf <<'EOF'
% Curriculum Vitae — Maëlle Delouis
Placeholder CV. Replace this file with your real CV PDF.
EOF
```

If PDF engine (LaTeX) is unavailable, instead create `assets/delouis-cv.pdf` from a minimal HTML print or leave a `assets/delouis-cv.pdf.README` instructing replacement; document the chosen fallback in the site README (Task 7). Verify a file exists at `assets/delouis-cv.pdf` OR the documented fallback.

- [ ] **Step 3: Verify assets exist**

Run: `ls -la assets/`
Expected: `profile.jpg` and `delouis-cv.pdf` present (or documented fallback).

- [ ] **Step 4: Commit**

```bash
git add assets/
git commit -m "feat: add placeholder headshot and CV PDF"
```

---

### Task 3: Apply minimal-academic theme (SCSS light + dark)

**Files:**
- Modify: `styles.scss`
- Modify: `styles-dark.scss`

**Interfaces:**
- Consumes: theme wiring from `_quarto.yml` (Task 1) referencing both SCSS files.
- Produces: a single `$accent` variable (top of `styles.scss`) controlling the site accent; serif headings + sans body applied site-wide.

- [ ] **Step 1: Write `styles.scss` (light theme)**

```scss
/*-- scss:defaults --*/

// === Single knob: change the whole-site accent here ===
$accent: #2c5f8a;   // muted slate/UZH-ish blue

// Fonts (pulled from Google Fonts by Quarto)
$font-family-sans-serif: "Inter", "Helvetica Neue", Arial, sans-serif;
$headings-font-family: "Lora", Georgia, "Times New Roman", serif;

$primary: $accent;
$link-color: $accent;
$body-color: #1f2328;
$body-bg: #ffffff;

/*-- scss:rules --*/

// Import fonts
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Lora:wght@500;600;700&display=swap');

body {
  font-size: 1.05rem;
  line-height: 1.65;
}

h1, h2, h3, h4 {
  letter-spacing: -0.01em;
}

// Generous whitespace, understated links, minimal chrome
main {
  max-width: 52rem;
}

a {
  text-decoration: none;
  &:hover { text-decoration: underline; }
}

.navbar {
  border-bottom: 1px solid #eceff3;
}

// Social/link icon row on the home page
.link-row a {
  margin-right: 0.9rem;
  font-size: 1.35rem;
  color: $accent;
}
```

- [ ] **Step 2: Write `styles-dark.scss` (dark theme)**

```scss
/*-- scss:defaults --*/

$accent: #7fb0d9;   // lighter accent for dark bg

$font-family-sans-serif: "Inter", "Helvetica Neue", Arial, sans-serif;
$headings-font-family: "Lora", Georgia, "Times New Roman", serif;

$primary: $accent;
$link-color: $accent;
$body-color: #d7dde3;
$body-bg: #16191d;

/*-- scss:rules --*/

@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Lora:wght@500;600;700&display=swap');

body {
  font-size: 1.05rem;
  line-height: 1.65;
}

main { max-width: 52rem; }

a {
  text-decoration: none;
  &:hover { text-decoration: underline; }
}

.navbar {
  border-bottom: 1px solid #23282e;
}

.link-row a {
  margin-right: 0.9rem;
  font-size: 1.35rem;
  color: $accent;
}
```

- [ ] **Step 3: Confirm dark toggle is enabled**

The `theme:` map with both `light:` and `dark:` in `_quarto.yml` (Task 1) auto-adds the navbar toggle. No change needed; just confirm it renders.

- [ ] **Step 4: Render and verify**

Run: `quarto render`
Expected: no errors. Open `_site/index.html` — headings are serif (Lora), body is sans (Inter), accent color visible on links, and a light/dark toggle appears in the navbar.

- [ ] **Step 5: Commit**

```bash
git add styles.scss styles-dark.scss
git commit -m "feat: minimal-academic theme with single accent variable and dark mode"
```

---

### Task 4: Build the Home / About page

**Files:**
- Modify: `index.qmd`

**Interfaces:**
- Consumes: `assets/profile.jpg`, `assets/delouis-cv.pdf` (Task 2); `.link-row` style (Task 3).
- Produces: the landing page. Uses Quarto's `about` template for a clean headshot + bio layout.

- [ ] **Step 1: Write `index.qmd` using the Quarto About template**

```markdown
---
title: "Maëlle Delouis"
about:
  template: trestles
  image: assets/profile.jpg
  image-shape: round
  links:
    - icon: envelope
      text: Email
      href: "mailto:maelle.delouis@uzh.ch"
    - icon: mortarboard
      text: Google Scholar
      href: "https://scholar.google.com/citations?user=PLACEHOLDER"
    - icon: person-badge
      text: ORCID
      href: "https://orcid.org/0000-0000-0000-0000"
    - icon: github
      text: GitHub
      href: "https://github.com/PLACEHOLDER"
    - icon: chat
      text: Bluesky
      href: "https://bsky.app/profile/PLACEHOLDER"
    - icon: linkedin
      text: LinkedIn
      href: "https://www.linkedin.com/in/PLACEHOLDER"
---

**PhD Student, Department of Sociology, University of Zurich**

<!-- Placeholder bio — replace with your own. -->

I am a PhD student in the Department of Sociology at the University of Zurich,
with research interests at the intersection of sociology and political science.
_This is placeholder text — replace it with a short description of your work._

My research focuses on _[placeholder research theme]_. I use _[placeholder
methods]_ to study _[placeholder questions]_. _Replace this paragraph with two
or three sentences about your substantive interests._

Before starting my PhD, I _[placeholder background — degrees, prior positions]_.
_Replace with your own background._

[Download CV](assets/delouis-cv.pdf){.btn .btn-outline-primary .btn-sm role="button"}
```

Note: the `about` template already renders the links as an icon row, so a separate `.link-row` div is not required on this page. Keep `.link-row` styling in case the owner wants a manual row later; it is harmless if unused.

- [ ] **Step 2: Render and verify**

Run: `quarto render index.qmd`
Expected: no errors. `_site/index.html` shows the round headshot, name, role line, bio paragraphs, the icon links, and a "Download CV" button.

- [ ] **Step 3: Commit**

```bash
git add index.qmd
git commit -m "feat: home/about page with headshot, bio, links, and CV button"
```

---

### Task 5: Build the Research page

**Files:**
- Modify: `research.qmd`

**Interfaces:**
- Produces: a hand-editable Markdown publications page with three grouped sections.

- [ ] **Step 1: Write `research.qmd`**

```markdown
---
title: "Research"
---

_Placeholder publications — replace each entry with your own. Formatting is
plain Markdown, so just edit the text._

## Peer-reviewed articles

- Delouis, M., & Coauthor, A. (2025). _Placeholder article title goes here._
  *Journal Name*, 00(0), 000–000.
  [PDF](#) · [DOI](https://doi.org/PLACEHOLDER)

- Delouis, M. (2024). _Another placeholder article title._
  *Journal Name*, 00(0), 000–000.
  [PDF](#) · [DOI](https://doi.org/PLACEHOLDER)

## Working papers

- Delouis, M. (2026). _Placeholder working-paper title._ Working paper.
  [PDF](#)

- Delouis, M., & Coauthor, B. (2025). _Another working-paper title._
  Under review at *Journal Name*.
  [PDF](#)

## Work in progress

- _Placeholder project title._ With Coauthor C. (data collection stage).

- _Another in-progress project title._ Solo-authored.
```

- [ ] **Step 2: Render and verify**

Run: `quarto render research.qmd`
Expected: no errors. `_site/research.html` shows three headed sections with the placeholder entries and working links.

- [ ] **Step 3: Commit**

```bash
git add research.qmd
git commit -m "feat: research page with grouped placeholder publications"
```

---

### Task 6: Build the CV page

**Files:**
- Modify: `cv.qmd`

**Interfaces:**
- Consumes: `assets/delouis-cv.pdf` (Task 2).
- Produces: a structured placeholder CV page with a prominent download button.

- [ ] **Step 1: Write `cv.qmd`**

```markdown
---
title: "Curriculum Vitae"
---

[Download PDF](assets/delouis-cv.pdf){.btn .btn-primary role="button"}

_Placeholder CV — replace each section with your own details._

## Education

- **PhD in Sociology**, University of Zurich — _2024–present (expected 20XX)_
- **MA in [Field]**, [University] — _20XX_
- **BA in [Field]**, [University] — _20XX_

## Positions

- **Doctoral Researcher**, Department of Sociology, University of Zurich — _2024–present_
- **[Prior role]**, [Institution] — _20XX–20XX_

## Publications

_See the [Research](research.qmd) page for a full list._

- Delouis, M. (2025). _Placeholder article title._ *Journal Name*.

## Teaching

- **[Course title]**, University of Zurich — _Semester, Year (role: TA/instructor)_
- **[Course title]**, University of Zurich — _Semester, Year_

## Awards & grants

- **[Grant / fellowship name]**, [Funder] — _Year_
- **[Award name]**, [Institution] — _Year_

## Service & activities

- Reviewer, *Journal Name*
- [Conference / workshop organization placeholder]
```

- [ ] **Step 2: Render and verify**

Run: `quarto render cv.qmd`
Expected: no errors. `_site/cv.html` shows the download button and all placeholder sections; the internal link to the Research page resolves.

- [ ] **Step 3: Commit**

```bash
git add cv.qmd
git commit -m "feat: CV page with download button and structured placeholders"
```

---

### Task 7: Add README with edit/run/deploy instructions

**Files:**
- Create: `README.md`

**Interfaces:**
- Produces: onboarding docs for the owner (local preview, editing, deploy, custom-domain-later).

- [ ] **Step 1: Write `README.md`**

````markdown
# Maëlle Delouis — Personal Website

Minimal academic website built with [Quarto](https://quarto.org) and deployed
to GitHub Pages.

## Edit content

- **Home / About:** `index.qmd`
- **Research (publications):** `research.qmd`
- **CV:** `cv.qmd` (and replace `assets/delouis-cv.pdf` with your real CV)
- **Headshot:** replace `assets/profile.jpg`
- **Accent color:** change `$accent` at the top of `styles.scss` (and
  `styles-dark.scss` for dark mode)
- **Links (email, Scholar, ORCID, GitHub, Bluesky, LinkedIn):** edit the
  `links:` block in `index.qmd`; delete any you don't use.

## Preview locally

```bash
quarto preview
```

Opens a live-reloading local server. Edit any `.qmd` and the browser updates.

## Deploy

Pushing to `main` triggers the GitHub Action (`.github/workflows/publish.yml`),
which renders the site and publishes it to the `gh-pages` branch.

**One-time setup:**
1. Create a GitHub repository and push this project to `main`.
2. In the repo: **Settings → Pages → Build and deployment → Source: Deploy
   from a branch → Branch: `gh-pages` / root**.
3. Wait for the first Action run to finish; your site is live at
   `https://<username>.github.io/<repo>/` (or `https://<username>.github.io/`
   if the repo is named `<username>.github.io`).

## Custom domain (later)

To use a custom domain (e.g. `maelledelouis.com`):
1. Add a `CNAME` file at the project root containing just your domain.
2. Configure the domain's DNS to point at GitHub Pages.
3. Set the domain under **Settings → Pages → Custom domain**.

See the [Quarto GitHub Pages guide](https://quarto.org/docs/publishing/github-pages.html).
````

Note: if a placeholder-asset fallback was used in Task 2, document that here (e.g. "replace the placeholder `assets/profile.jpg`").

- [ ] **Step 2: Commit**

```bash
git add README.md
git commit -m "docs: add README with edit, preview, and deploy instructions"
```

---

### Task 8: Add GitHub Actions deploy workflow

**Files:**
- Create: `.github/workflows/publish.yml`

**Interfaces:**
- Consumes: the whole Quarto project.
- Produces: automated render + publish to `gh-pages` on push to `main`.

- [ ] **Step 1: Create the workflow**

```yaml
name: Publish

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: write

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository
        uses: actions/checkout@v4

      - name: Set up Quarto
        uses: quarto-dev/quarto-actions/setup@v2
        with:
          version: 1.7.29

      - name: Render and publish to GitHub Pages
        uses: quarto-dev/quarto-actions/publish@v2
        with:
          target: gh-pages
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

Note: no R or Python setup step is needed — content is static Markdown with no
executed code. If executed code is added later, add the matching setup step.

- [ ] **Step 2: Verify workflow YAML is valid locally**

Run: `quarto render` (confirms the project still builds; the Action runs the same render).
Expected: no errors.

Optionally validate YAML syntax:
Run: `python3 -c "import yaml,sys; yaml.safe_load(open('.github/workflows/publish.yml')); print('ok')"`
Expected: `ok`

- [ ] **Step 3: Commit**

```bash
git add .github/workflows/publish.yml
git commit -m "ci: publish to GitHub Pages via Quarto Action on push to main"
```

- [ ] **Step 4: Final full-site verification**

Run: `quarto render`
Expected: no errors; `_site/` contains `index.html`, `research.html`, `cv.html`
with styling applied. Optionally run `quarto preview` and click through all
three pages, the dark-mode toggle, the CV download button, and the home-page
links.

---

## Post-implementation (owner actions, not in scope for the agent)

- Create the GitHub repo and push `main` (the agent should not create remote
  repos or push unless the owner asks).
- Enable GitHub Pages per the README.
- Replace placeholder photo, CV PDF, bio, publications, and links.

## Notes on adaptation from standard TDD

This is a static site, so tasks use `quarto render`/`quarto preview` as the
verification cycle instead of unit tests. Each task still ends with an
independently verifiable, committed deliverable.
