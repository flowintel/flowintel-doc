---
title: "Deployment and configuration"
---

Flowintel combines a Flask application, a relational database, Valkey/Redis sessions, a frontend asset build, and optional services such as MISP modules. A working web process alone does not confirm that reminders, enrichment or synchronisation are running.

## Configuration files

| File | Purpose |
|---|---|
| `conf/config.py` | Application settings, database/session configuration, authentication and feature settings. |
| `conf/config_module.py` | Connector origin URL, notification delivery and module settings. |
| `.env` or service environment | Runtime values such as database credentials, `SECRET_KEY` and initial account settings. |
| `docker/docker-compose*.yml` | Container services, networks, volumes and configuration mounts. |

Copy configuration from the `.default` files for a fresh installation. For an existing installation, compare new defaults with your local settings instead of overwriting them.

## Settings to review

- `SECRET_KEY`: set a unique, persistent secret for the instance.
- `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER`, `DB_PASSWORD`, `DB_DIALECT`, `DB_DRIVER`: select and connect to the database used by your runtime configuration.
- `VALKEY_IP`, `VALKEY_PORT`, `VALKEY_PASSWORD`: connect to session storage.
- `ADMIN_MAIL`, `ADMIN_PASSWORD`: configure the initial account before database initialisation; these are not a password-reset mechanism for existing users.
- `FLOWINTEL_APP_HOST`, `FLOWINTEL_APP_PORT`: select the application bind address and port.
- `MISP_MODULE_HOST`, `MISP_MODULE_PORT`: locate the enrichment service.
- `ORIGIN_URL` in module configuration: set the origin used in exported links.
- `MISP_EXPORT_FILES`: explicitly decide whether MISP exports include attachments.

The launcher selects development or production mode. `-l` is development; `-p` runs Gunicorn with production configuration. A Docker deployment selects its environment through Compose and the Docker launch path.

## Native production installation

Prepare the database, runtime credentials and service account before initialisation. With configuration in place, the safe installer supports:

```bash
bash install.safe.sh --production
```

Production initialisation loads reference data without the development sample cases. Build frontend assets as described in [Getting started]({{< relref "installation" >}}), then configure process supervision, HTTPS and a reverse proxy for shared access.

The application repository provides an [installation manual](https://github.com/flowintel/flowintel/blob/main/doc/installation-manual.md), NGINX example and systemd service examples under `doc/`. Adapt their paths, environment and service users to your host. Do not run duplicate launcher workers and independently supervised workers for the same job.

## Container variants

The published-image Compose file is `docker/docker-compose.yml`. Local-image variants are supplied for PostgreSQL and MariaDB:

| Variant in filename | Services supplied |
|---|---|
| `full` | Flowintel, database and Valkey |
| `infra` | Database and Valkey for an application running on the host |
| `nodb` | Flowintel and Valkey, with an external database |
| `single` | Flowintel only, with external dependencies |

Local variants expect a locally built image. Run `make help` in the application repository to inspect build and deployment targets. For example, `make build_latest_local` builds the local image; use the matching Compose variant and environment template for your database.

Treat the supplied stacks as starting configurations. Review image versions, network ranges, port bindings, secrets and storage before deploying. `latest` is mutable; record the deployed image identity so upgrades can be reproduced.

## Persistent data

Preserve the database and the files required to interpret it: uploads, configuration, history, logs where required, and any signing/encryption keys. Compose uses named volumes for several of these locations and bind mounts for configuration.

Stopping containers with `docker compose down` normally retains named volumes. Adding `--volumes`, or invoking volume-removal Make targets, deletes persistent storage. Do not use those operations as an upgrade procedure.

Continue with [Operations and backups]({{< relref "operations" >}}) before accepting production data.
