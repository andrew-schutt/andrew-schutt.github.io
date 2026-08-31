# andrew-schutt.github.io

My personal site — [andrew-schutt.com](https://andrew-schutt.com) — a static site
built with [Jekyll](https://jekyllrb.com/) and deployed to GitHub Pages via GitHub
Actions.

## Requirements

- **Ruby 4.x** — the version is pinned to `4.0.2` in [`.ruby-version`](.ruby-version)
  (use `rbenv`, `asdf`, or similar to match it).
- **Bundler** — a modern release (the lockfile is bundled with `4.0.19`). If the
  system `bundle` errors on Ruby 4, install a current one with `gem install bundler`.
- **Network access on the first build** — the site uses a `remote_theme`
  ([StartBootstrap/startbootstrap-clean-blog-jekyll](https://github.com/StartBootstrap/startbootstrap-clean-blog-jekyll)),
  which Jekyll fetches from GitHub at build time.

No Node.js or other toolchain is required.

## Running locally

```sh
bundle install                # install gems (first run only)
bundle exec jekyll serve      # serve with live reload at http://localhost:4000
```

To produce a one-off build into `_site/` without serving:

```sh
bundle exec jekyll build
```

## Deployment

Pushing to `main` deploys automatically. The site is built and published by the
GitHub Actions workflow at [`.github/workflows/jekyll.yml`](.github/workflows/jekyll.yml),
which runs `jekyll build` on **Ruby 4** and publishes the result to GitHub Pages.
It triggers on every push to `main` and can also be run manually
(`workflow_dispatch`).

Notes:

- The repository's **Pages source is set to "GitHub Actions"** (not "Deploy from a
  branch"). This is what lets the site build on modern Jekyll 4 / Ruby 4 instead of
  GitHub's legacy built-in Jekyll (which pins Jekyll 3.9 and Ruby < 4.0). The repo
  intentionally does **not** use the `github-pages` gem for this reason.
- The custom domain lives in [`CNAME`](CNAME) (`andrew-schutt.com`) and is copied
  into the build as-is.
- There is no local SCSS; all styles come from the remote theme.

## Project structure

- `index.markdown`, `about.html`, `contact.html` — top-level pages.
- `_posts/` — blog posts (Markdown, rendered with kramdown).
- `_includes/navbar.html` — local override of the theme's nav bar.
- `slides/` — self-hosted presentations (see below).
- `_config.yml` — site configuration, plugins, and Sass settings.

## Adding a talk (slide deck)

Presentations are self-hosted [reveal.js](https://revealjs.com/) decks exported
from slides.com (**Export → HTML**), so they keep working independent of any
slide-hosting service.

1. Unzip the export into `slides/<deck-name>/` using a URL-friendly folder name.
   Copy the bundle **verbatim** — its `index.html` has no YAML front matter, so
   Jekyll serves it untouched and reveal.js (including the **S**-key speaker view
   with notes) keeps working.
2. Add a link to the deck in [`slides/index.html`](slides/index.html).
3. Make sure the deck's reveal.js config has `showNotes: false` so speaker notes
   stay private to the S-key view rather than rendering inline for every visitor.

Do not commit the source `.zip` exports; only the unzipped deck folders belong in
the repo.
