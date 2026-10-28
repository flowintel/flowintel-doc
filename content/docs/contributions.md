---
title: "Contributing"
---

Flowintel's [application repository](https://github.com/flowintel/flowintel) contains the product source and detailed manuals. The [documentation repository](https://github.com/flowintel/flowintel-doc) contains this Hugo website.

## Improve the documentation

Open an issue or pull request with the affected page, the behaviour in your application version and the proposed correction. Useful contributions include reproducible setup instructions, practical workflows, API examples and clearer troubleshooting guidance.

When editing this site:

1. Change Markdown in `content/` and add navigation entries in `hugo.toml` when introducing a page.
2. Use Hugo `relref` links for internal pages so links work under the GitHub Pages subpath.
3. Put screenshots in `static/images/`, remove sensitive content, and supply meaningful alternative text.
4. Build with `hugo --minify`, check links and headings, and preview with `hugo server`.
5. Keep generated `public/` output out of content changes.

## Contribute to the application

Follow the application repository's `AGENTS.md`, [contributor guide](https://github.com/flowintel/flowintel/blob/main/doc/contributor.md) and [technical specifications](https://github.com/flowintel/flowintel/blob/main/doc/technical-specifications.md).

The backend uses Flask and SQLAlchemy; frontend dependencies are built with Vite. Tests are organised by feature under `tests/`. Run checks relevant to the changed feature and build frontend assets when changing their sources.

Use [issues](https://github.com/flowintel/flowintel/issues) for reproducible bugs and feature proposals, and [pull requests](https://github.com/flowintel/flowintel/pulls) for changes. For a suspected security vulnerability, follow the application's [security policy](https://github.com/flowintel/flowintel/blob/main/SECURITY.md).
