---
title: "Getting started"
---

Use this guide for a local evaluation. For a shared installation, continue with [Deployment and configuration]({{< relref "deployment" >}}). Commands below run from the **Flowintel application repository**, not the documentation repository.

## Requirements

- Git and a Linux environment supported by the installation scripts (`apt` or `dnf`), with permission to install system packages.
- Python compatible with the application checkout. The current `pyproject.toml` declares Python **3.10 to 3.13**; Python 3.12 is a suitable starting point.
- Valkey or Redis for sessions, plus SQLite for local evaluation or PostgreSQL/MySQL/MariaDB for a configured database deployment.
- Bun for frontend dependencies and the Vite asset build. Document exports also use Pandoc, LaTeX and Mermaid tools.

The installer changes system packages and installs tools in your home directory. Review it before running it. Package availability depends on your distribution.

## Install

```bash
git clone https://github.com/flowintel/flowintel.git
cd flowintel
cp conf/config.py.default conf/config.py
cp conf/config_module.py.default conf/config_module.py
```

Review both configuration files. Set a unique `SECRET_KEY` in your runtime environment and configure Valkey connectivity. The supplied configuration reads environment variables; keep local settings and credentials out of version control.

```bash
bash install.safe.sh
```

The default mode creates `env/`, installs dependencies, initialises submodules and the development database, loads taxonomies and galaxies, registers analysis modules, and adds sample cases. Check the output for failures: some setup steps report warnings without stopping the installer.

The safe installer does not build the frontend assets. With Bun installed, build them separately:

```bash
cd app/assets
bun install
bun run build:static
cd ../..
```

The repository also contains `install.sh`, which uses uv and Bun. These are alternative bootstrap scripts; do not run both as successive installation steps.

## Run

```bash
bash launch.sh -l
```

Open `http://127.0.0.1:7006` unless you changed the host or port. This launches development mode and background workers. Use `Ctrl+C` to stop the foreground process; `bash launch.sh -ks` stops the launcher's managed background sessions.

## First login

If you kept the initial account defaults, sign in with:

- Email: `admin@admin.admin`
- Password: `admin`

If you supplied `ADMIN_MAIL` and `ADMIN_PASSWORD` before database initialisation, use those credentials instead. Change default credentials before allowing other people to reach the service.

Create your organisations and users, review their permissions, then follow [Your first investigation]({{< relref "cases-tasks" >}}). The sample cases are for exploration; use a separate deployment for operational data.

## Run with Docker

The Compose files live under `docker/`. Configuration files must exist because Compose mounts them into the container. After cloning and copying the configuration files as above:

```bash
cp template.env .env
```

Edit `.env`: replace placeholder secrets, set initial administrator credentials, and use `DB_HOST=postgresql` and `VALKEY_IP=valkey` for the bundled services. Then inspect and start the published-image stack:

```bash
docker compose --env-file .env -f docker/docker-compose.yml config --quiet
docker compose --env-file .env -f docker/docker-compose.yml up -d
docker compose --env-file .env -f docker/docker-compose.yml logs -f flowintel
```

This stack includes PostgreSQL, Valkey and `ghcr.io/flowintel/flowintel:latest`. The default host binding is loopback on port 7006. For deployment variants and persistent data, see [Deployment]({{< relref "deployment" >}}).

## Next steps

- [Cases, tasks and evidence]({{< relref "cases-tasks" >}}): work through an investigation.
- [Templates and notes]({{< relref "templates" >}}): standardise repeatable work.
- [Administration]({{< relref "administration" >}}): configure access and review workflows.
- [Troubleshooting]({{< relref "troubleshooting" >}}): diagnose setup and integration problems.

For architecture and data-model details, see the [technical specifications](https://github.com/flowintel/flowintel/blob/main/doc/technical-specifications.md).
