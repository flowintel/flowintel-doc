---
title: "Connectors vs Analysers"
weight: 40
---

Connectors and analysers are not the same, but they share some features that can
be confusing.

![](/images/flowintel_connections.png)

Analysers receive data from Flowintel and can send data back to enrich the notes
of a case or a task.

Connectors can only receive data from Flowintel. For MISP, this creates an event
with a `flowintel-cm-case` object for a case and a `flowintel-cm-task` object for
a task.
