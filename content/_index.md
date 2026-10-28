---
title: "Flowintel documentation"
description: "Case management and collaborative investigations for security analysts."
---

Flowintel is an open-source case management platform for security analysts. Organise investigations, assign work, preserve evidence and exchange threat intelligence through a shared workspace.

## Start with your task

| I want to… | Read |
|---|---|
| Try Flowintel locally | [Getting started]({{< relref "installation" >}}) |
| Run an investigation | [Cases, tasks and evidence]({{< relref "cases-tasks" >}}) |
| Create repeatable procedures | [Templates and notes]({{< relref "templates" >}}) |
| Manage users and review permissions | [Administration and access]({{< relref "administration" >}}) |
| Connect another tool | [Choose an integration]({{< relref "connectors-analyzers" >}}) |
| Automate work | [REST API]({{< relref "api" >}}) |
| Operate a shared instance | [Deployment]({{< relref "deployment" >}}) and [Operations]({{< relref "operations" >}}) |

## What you can do

- **Manage cases and tasks:** organise work by case, organisation, assignee, status and deadline, with subtasks and external references.
- **Document evidence:** write Markdown notes, add Mermaid diagrams, attach files and generate reports.
- **Build shared procedures:** reuse case, task and note templates, including templates from a central repository.
- **Control access and review:** combine roles, organisation membership, private cases and privileged task approval workflows.
- **Add intelligence context:** use MISP taxonomies, galaxies, objects and standalone attributes, then enrich evidence with MISP modules.
- **Exchange and triage data:** send and receive through connectors, configure MISP synchronisation, import cases and review incoming alerts.
- **Optional assistant and single sign-on:** configure the Ollama-backed assistant and supported identity providers for your deployment.
- **Follow activity:** use assignments, calendar views, reminders, notifications, case history and audit logs.

![An investigation with its tasks in Flowintel](/images/current/case-overview.jpg)

## About these guides

These guides describe the application source reviewed in October 2026. Available features depend on your installed version, permissions and configuration. The detailed manuals linked in the sidebar provide further deployment and product reference; your instance's `/api/` page describes its API.

## Licence

Flowintel is licensed under the [GNU Affero General Public License version 3](https://www.gnu.org/licenses/agpl-3.0.html).
