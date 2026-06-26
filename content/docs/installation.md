---
title: "Getting started"
weight: 10
---

This quick start runs Flowintel in **development mode** (an SQLite database, with
sample cases). For a production setup with PostgreSQL, NGINX, HTTPS and systemd
services, follow the full
[installation manual](https://github.com/flowintel/flowintel/blob/main/doc/installation-manual.md).

## Install

Clone the repository:

```bash
git clone https://github.com/flowintel/flowintel.git
cd flowintel
```

### Configuration

Copy the default configuration files and adjust them to your environment:

```bash
cp conf/config.py.default conf/config.py
cp conf/config_module.py.default conf/config_module.py
```

The main settings live in `conf/config.py`. If you plan to use modules, such as
connectors, analysers or notifications, review `conf/config_module.py` as well.

### Run the installation script

The script creates a Python virtual environment, installs the dependencies, sets
up the database and loads the sample data:

```bash
bash install.safe.sh
```

It takes around 10 to 15 minutes and asks for your `sudo` password along the way.
For a production install on PostgreSQL, run `bash install.safe.sh --production`
after setting up the database as described in the installation manual.

## Run

```bash
bash launch.sh -l
```

Flowintel listens on `http://127.0.0.1:7006` by default. Press `Ctrl+C` to stop it.

## Run with Docker

You can also start Flowintel together with its dependencies (PostgreSQL and
Valkey) using Docker Compose:

```bash
docker compose up -d
```

This pulls the published `ghcr.io/flowintel/flowintel:latest` image and serves
Flowintel on port `7006`.

## First login

Open Flowintel in your browser and sign in with the default administrator
account:

- **Email**: `admin@admin.admin`
- **Password**: `admin`

> [!WARNING]
> Change the administrator password straight after the first login, from your
> user profile.

## Next steps

- The [installation manual](https://github.com/flowintel/flowintel/blob/main/doc/installation-manual.md) covers production setups, single sign-on, MISP and report signing.
- The [user manual](https://github.com/flowintel/flowintel/blob/main/doc/user-manual.md) explains how to configure Flowintel and handle cases day to day.
- The [encryption guide](https://github.com/flowintel/flowintel/blob/main/doc/encryption-guide.md) shows how to protect data at rest.
- The [backup and restore guide](https://github.com/flowintel/flowintel/blob/main/doc/backup-restore.md) explains how to back up your data and restore it.
