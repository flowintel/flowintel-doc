---
title: "Operations, upgrades and backups"
---

## Check the whole service

Verify login, opening a case, creating a task, storing an attachment and reading it back. If enabled, also test a reminder, an analyser call, and a connector exchange using non-sensitive test data.

The launch scripts manage background processes as well as the web application. Useful diagnostics from the application directory include:

```bash
screen -ls
tail -n 100 logs/fcm.log
tail -n 100 logs/misp_sync.log
tail -n 100 logs/misp.log
```

For systemd or container installations, inspect the corresponding service or container logs instead. The relevant background sessions in launcher-based installations are `fcm` for recurring notifications and `misp_sync` for MISP synchronisation.

## Back up a complete installation

A case JSON export is useful for sharing, but is not an instance backup. A recoverable backup includes:

- A consistent database backup using the tools for the selected database engine.
- Attachments and uploads, case history, and logs required by your retention policy.
- Application and module configuration, runtime secrets, and any GPG or encryption keys needed after recovery.
- The application revision or image identity and the dependency/runtime configuration used with that database.

Store a copy outside the application host and test restoration in an isolated environment. The repository's [backup and restore guide](https://github.com/flowintel/flowintel/blob/main/doc/backup-restore.md) provides a native PostgreSQL example and scheduling instructions. Its paths and retention periods are examples to adapt; backups are not enabled simply by installing Flowintel.

For encrypted storage and key recovery, consult the [encryption guide](https://github.com/flowintel/flowintel/blob/main/doc/encryption-guide.md).

## Upgrade deliberately

1. Record the current revision/image and review the intended update.
2. Take a database and filesystem backup, and confirm that you can restore it.
3. Test the upgrade with a copy of the data and the same database backend.
4. Stop or supervise application processes and workers during migration.
5. Update dependencies, database migrations, frontend assets and reference data as required by the chosen release.
6. Restart and verify the web application, workers, attachments and integrations.

`update.sh --env production` is a convenience script, not a backend-independent deployment tool. It pulls Git changes, backs up PostgreSQL, invokes migrations, updates Python dependencies and reference data, and stops managed sessions. Its non-development backup path assumes PostgreSQL; adapt the process for MariaDB/MySQL and container deployments. It does not replace a complete backup or a frontend asset build.

Do not use `launch.sh -r` for an upgrade: it recreates the database.

## Reference data and modules

The launcher provides commands to refresh reference data:

```bash
bash launch.sh -tg
bash launch.sh -mm
```

Use `-tg` for taxonomies and galaxies and `-mm` for MISP module registration. These launcher operations can interact with background services; run them as planned maintenance and verify normal services afterwards.

Custom taxonomies belong in `modules/custom_taxonomies/` with a `MANIFEST.json`. Custom galaxies belong in `modules/custom_galaxies/` with `clusters/` and `galaxies/` directories.
