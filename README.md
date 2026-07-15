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

## Placeholder assets

Both files under `assets/` are placeholders and should be replaced before the
site is considered final:

- `assets/profile.jpg` — a generated placeholder headshot (produced with
  Python/PIL, a plain neutral square). Replace with your real photo.
- `assets/delouis-cv.pdf` — a placeholder CV PDF generated via Quarto's Typst
  engine (a real, valid PDF, but with placeholder text only). Replace with
  your actual CV PDF.
