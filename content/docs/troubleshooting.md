---
title: "Troubleshooting"
---

Start with the failing operation and the corresponding service log. Record your application revision, deployment mode and database backend when reporting a problem. Remove credentials, API keys and sensitive case data from diagnostic output.

## The web page does not load

Check the process or container logs and the configured host/port. The default local address is `http://127.0.0.1:7006`; a loopback binding is not reachable from another computer. For a shared deployment, also check the reverse proxy and its upstream address.

## Login or sessions fail

Confirm that the database and Valkey are reachable from the application runtime. In a container, `127.0.0.1` refers to that container, not the database or Valkey service. Check runtime credentials and a persistent `SECRET_KEY`.

Use the administrator credentials chosen at database initialisation. Changing `ADMIN_PASSWORD` after initialisation does not reset an existing account. See [account recovery]({{< relref "administration" >}}#account-setup-and-recovery).

## The page loads but interface assets are missing

Build assets from `app/assets` using `bun install` and `bun run build:static`, then check that your runtime serves the generated static files. The safe installer does not perform this build. If the build fails, check the JavaScript runtime against the installed dependency requirements rather than assuming an older installer-pinned Node version is sufficient.

## An action is missing or returns permission denied

Check the user's role, specialised permissions, organisation membership and case visibility. Private cases restrict access; privileged cases restrict actions while tasks await review. API keys act with their owner's permissions.

## Analysers are empty or fail

Check that MISP modules is running at the configured host and port, then refresh module registration with `bash launch.sh -mm`. A module may require an API key, network access or an input type different from the selected value. Review `logs/misp.log` and the module result before retrying.

## MISP sends or receives fail

Check the connector URL, user-specific credentials, module direction and event identifier. Read-only instances block sends. Inspect the connector's sync status and conflicts before assuming a failed operation should simply be repeated. For automated runs, check the `misp_sync` worker and `logs/misp_sync.log`.

## Recurring reminders do not arrive

The recurring worker must be running at its scheduled time, currently **03:00 server-local time**. Check the case's recurring date/type and whether it is completed, then inspect `logs/fcm.log`. External delivery additionally requires the email, Matrix or webhook configuration to work.

## Import fails

Use a JSON export from a compatible Flowintel version as the starting point. Check duplicate titles, date formats, tag availability and whether you selected case import or template import. For batch imports, inspect existing cases before retrying: earlier entries may already have been created.

## PDF or document export fails

Check Pandoc, the relevant LaTeX engine/templates, and Mermaid rendering dependencies in the service user's environment. A command available in your interactive shell may be absent from a systemd service's `PATH`. Test a short text-only note first, then a document with diagrams.

## Report a reproducible issue

Include the expected result, actual result, minimal reproduction steps, application revision, deployment method and relevant sanitised log excerpt in a [Flowintel issue](https://github.com/flowintel/flowintel/issues).
