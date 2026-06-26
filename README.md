# flowintel-doc

Documentation website for [flowintel](https://github.com/flowintel/flowintel),
built with [Hugo](https://gohugo.io/) and a custom theme matching the Flowintel
application branding.

The site is published to GitHub Pages at
<https://flowintel.github.io/flowintel-doc/>.

## Requirements

- [Hugo](https://gohugo.io/installation/) `v0.155+`

## Local development

Run a live-reloading local server:

```bash
hugo server
```

Then open <http://localhost:1313/flowintel-doc/>.

## Build

```bash
hugo --minify
```

The generated site is written to `public/`.

## Structure

```
content/            Markdown content
  _index.md         Home page
  docs/             Documentation pages
themes/flowintel/   Custom theme (layouts, CSS, fonts)
static/             Images and favicon (served at the site root)
hugo.toml           Site configuration and left-sidebar navigation menu
```

Navigation is defined by the `[menu]` section in `hugo.toml`. The Documentation
links in the sidebar and the manual buttons on the home page come from the
`[[params.manuals]]` list in the same file.
