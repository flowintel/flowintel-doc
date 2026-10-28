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

## Content maintenance

The October 2026 refresh was checked against the local `flowintel` checkout at
commit `88503112` and its working-tree source. It is a source-based documentation
refresh, not a claim that every deployment runs those features. Existing page
URLs are preserved; no CSS or theme templates are changed.

Check product claims against the application code as well as its manuals:

| Website content | Application reference |
|---|---|
| Installation and deployment | `install.safe.sh`, `launch.sh`, `pyproject.toml`, `conf/*.default`, `docker/` |
| Cases, templates and administration | `doc/user-manual.md`, `app/case/`, `app/templating/`, `app/decorators.py` |
| API reference and examples | `app/api.py` and feature `*_api.py` routes |
| Import format | `app/tools/tools_core.py`, `app/utils/jsonschema_flowintel.py` |
| Connectors and synchronisation | `app/modules/`, `app/connectors/`, `app/bin/recurring_misp_sync.py` |
| Reminders and incoming alerts | `app/bin/recurring_notification.py`, `app/alerting/` |
| Backup guidance | `doc/backup-restore.md`, `update.sh` and the deployed storage layout |

Keep executable examples small and use placeholders or environment variables for
credentials. Verify methods, payload fields and response fields before changing
API examples. Prefer links to the deployed Swagger UI over copying a full endpoint
catalogue. Use Hugo `relref` for internal page links and check rendered anchors.
Current documentation screenshots are stored in `static/images/current/`. They
were captured from the local Flowintel v4.0.0 interface on 28 October 2026 at
1440 × 1000, without changing case or template data. Analyser captures use saved
runs because the MISP-modules service was unreachable during capture. The old
selection screenshot is no longer embedded; recapture that step when the service
is available. Older image files are retained to avoid removing existing assets.
