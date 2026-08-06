---
title: "flowintel"
description: "Helps analysts organise their cases and tasks."
---

## Features

- **Cases and tasks**: built for security analysts to track and organise their work.

- **Private and privileged cases**: limit who can see a case, and enforce a four-eyes review before tasks are completed.

- **Notes**: write detailed notes in Markdown, add Mermaid diagrams, and export them to formats such as PDF.

- **Reports**: generate a case report in Markdown or PDF, optionally signed with a GPG key so recipients can verify it.

- **MISP standard**: works with [MISP taxonomies](https://github.com/MISP/misp-taxonomies) and the [MISP galaxy](https://www.misp-galaxy.org/).

- **Calendar and notifications**: a calendar view and notifications help you keep track of your tasks.

- **Alerting**: outbound webhooks push events, such as the creation of a case, to your own systems.

- **Templates**: reusable templates for cases and tasks, so you can build a library of playbooks.

- **Data export**: modules to send data to other tools, such as [MISP](https://www.misp-project.org/) and [AIL](https://www.ail-project.org/).

- **API**: work with Flowintel from your own scripts.

- **Analysis modules**: run MISP modules to enrich your data and pull in threat intelligence.

- **Chatbot**: an optional assistant, backed by your own [Ollama](https://ollama.com/) server, that queries Flowintel through its API.

- **Single sign-on**: authenticate with Keycloak or Microsoft Entra ID, and map their groups to Flowintel roles.

- **Access control and audit trail**: roles and granular permissions per organisation, with a record of every action on a case.

![task-management](/images/case_example.png)

## Licence

This software is licensed under the [GNU Affero General Public License version 3](http://www.gnu.org/licenses/agpl-3.0.html).

```
Copyright (C) 2022-2023 CIRCL - Computer Incident Response Center Luxembourg
Copyright (C) 2022-2023 David Cruciani
```
