---
title: "Choose an integration"
---

Choose the workflow based on what you want to achieve. Connectors support both sending and receiving; they are not limited to export.

| Goal | Workflow | Result |
|---|---|---|
| Enrich a domain, IP address, hash or other attribute | [Analysers]({{< relref "analyzers" >}}) | Review module results and save selected information into an investigation. |
| Exchange data with a known MISP event | [Connectors]({{< relref "connectors" >}}) | Link a case/task to an instance and send or receive through a module. |
| Keep a linked case and MISP event updated | [MISP synchronisation]({{< relref "connectors" >}}#automated-synchronisation-and-conflicts) | Schedule exchanges and review conflicts. |
| Move cases between Flowintel instances | [Import and export]({{< relref "importer" >}}) | Transfer Flowintel JSON. |
| Start an investigation from a MISP event | Create case from MISP | Select event content and a case template. |
| Receive events for triage from an external tool | [Incoming alerts]({{< relref "alerts" >}}) | Review an alert before creating a case. |
| Automate case and task management | [REST API]({{< relref "api" >}}) | Work with the permissions of an API-key owner. |

![Flowintel integration overview](/images/flowintel_connections.png)

An analyser answers a question about submitted data. A connector maintains the relationship to a particular external platform or event. A typical investigation uses both: enrich an indicator, save the useful result, then share selected intelligence through a connector.
