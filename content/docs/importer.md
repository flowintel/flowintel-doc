---
title: "Import and export"
---

Use Flowintel JSON to move cases and templates between instances. Use the MISP-specific workflows when your input is a MISP event. These formats are not interchangeable.

## Import a case

1. Download a case from the source Flowintel instance, or prepare compatible JSON.
2. Open the importer and select case import. Case import requires Importer permission or administrator access.
3. Submit the JSON and read the result.
4. Open the imported case and review its tasks, context and evidence before using it operationally.

The API equivalent is `POST /api/importer/case`. It accepts a single case object or a list of case objects.

## Current case JSON format

The importer expects export-style fields, including fields whose values are empty. A title-only payload used for API case creation is not sufficient for case import. The example below includes a task, notes, a subtask, a URL, a MISP object, a standalone attribute and links between them. The domain and IP address are examples.

Use a current case download as the starting point for your own data. Keep `tasks`, `misp-objects` and `standalone_attributes` as arrays; use `[]` when there are no entries. Notice the hyphen in `misp-objects` and the underscore in `standalone_attributes`.

```json
{
  "title": "Domain investigation",
  "description": "Review a reported domain and record the findings.",
  "uuid": "0b1f9a85-0d38-46a1-b9dd-1eeea1608308",
  "time_required": "",
  "is_private": false,
  "ticket_id": "",
  "deadline": null,
  "recurring_date": null,
  "recurring_type": null,
  "notes": "# Investigation\nRecord scope and conclusions here.",
  "hedgedoc_url": "",
  "tags": [],
  "clusters": [],
  "custom_tags": [],
  "tasks": [
    {
      "title": "Triage the report",
      "description": "Confirm the report and document supporting evidence.",
      "uuid": "ddd271b4-d7f8-4af0-a9b3-46ad52aca1bf",
      "time_required": "",
      "deadline": null,
      "notes": [
        {
          "note": "# Findings\n"
        }
      ],
      "subtasks": [
        {
          "description": "Check the reported domain."
        }
      ],
      "urls_tools": [
        {
          "name": "https://example.org/report"
        }
      ],
      "tags": [],
      "clusters": [],
      "custom_tags": [],
      "misp_object_links": [
        {
          "misp_object_template_uuid": "43b3b146-77eb-4931-b4cc-b66c60f28734",
          "misp_object_name": "domain-ip"
        }
      ],
      "misp_attribute_links": [
        {
          "misp_attribute_uuid": "2dba9860-8706-4c86-862e-ad5e8df7be30"
        }
      ]
    }
  ],
  "misp-objects": [
    {
      "uuid": "32ba07e0-d16f-4f99-8fbd-c08d1a023e3c",
      "template_uuid": "43b3b146-77eb-4931-b4cc-b66c60f28734",
      "name": "domain-ip",
      "attributes": [
        {
          "uuid": "b8316616-44f0-4dd9-a165-67ea880ea869",
          "value": "example.org",
          "type": "domain",
          "object_relation": "domain",
          "first_seen": "",
          "last_seen": "",
          "comment": "Reported domain.",
          "ids_flag": false,
          "disable_correlation": false
        }
      ],
      "references": [
        {
          "referenced_type": "attribute",
          "referenced_attribute_uuid": "2dba9860-8706-4c86-862e-ad5e8df7be30",
          "relationship_type": "related-to",
          "comment": "Address associated with the reported domain."
        }
      ]
    }
  ],
  "standalone_attributes": [
    {
      "uuid": "2dba9860-8706-4c86-862e-ad5e8df7be30",
      "value": "192.0.2.10",
      "type": "ip-dst",
      "object_relation": "",
      "first_seen": "",
      "last_seen": "",
      "comment": "Example address from the report.",
      "ids_flag": false,
      "disable_correlation": false
    }
  ]
}
```

### Case and task fields

| Field | Format and behavior |
| --- | --- |
| `title`, `description` | Strings. Choose a unique case title: an existing title causes rejection. |
| `uuid` | Include a string for both the case and each task. Import creates new case/task UUIDs; it does not update existing records. |
| `time_required`, `ticket_id` | Strings; use `""` when empty. `ticket_id` belongs to the case. |
| `is_private` | Case visibility, expressed as a JSON boolean. |
| `deadline` | Include for both cases and tasks. Use `null` or `""` for no deadline, or `YYYY-MM-DD HH:MM`, for example `2026-11-05 14:30`. |
| `recurring_date`, `recurring_type` | Include both case fields. Leave both `null` for a non-recurring import; see the recurrence limitation below. |
| Case `notes` | One Markdown string, or `""`. |
| Task `notes` | An array of objects containing a `note` Markdown string. |
| `hedgedoc_url` | Optional case HedgeDoc URL; the linked document is not embedded in the JSON. |
| `tags` | An array of taxonomy tag names. Tags must exist on the destination instance or import is rejected. |
| `clusters` | An array of objects containing `name`. Matching clusters must already exist on the destination. |
| `custom_tags` | An array of names or objects containing `name`. Matching custom tags must already exist on the destination. Unknown clusters/custom tags are not created automatically. |
| Task `subtasks` | An array of objects containing `description`. Imported subtasks are created anew. |
| Task `urls_tools` | An array of objects containing `name`, holding the URL or tool text. |

### Evidence and task links

`misp-objects` contains objects with a `name`, a `template_uuid` and an `attributes` array. Use the MISP object template's UUID, not the individual object's UUID, for `template_uuid`. Each object attribute needs `value`, `type` and `object_relation`. Optional fields include `uuid`, `first_seen`, `last_seen`, `comment`, `ids_flag` and `disable_correlation`.

`standalone_attributes` contains attributes outside an object. Each needs a non-empty `value` and `type`; use an empty `object_relation`. The importer also accepts the older `misp-attributes` key. Prefer `standalone_attributes` and avoid supplying both: a non-empty canonical list takes precedence, while an empty or absent canonical list falls back to the alias.

Task evidence links are separate from the evidence itself:

- `misp_object_links` matches `misp_object_template_uuid` and `misp_object_name` against an imported object's `template_uuid` and `name`. Multiple objects with the same template and name can therefore match the same task link.
- `misp_attribute_links` restores links to standalone attributes by matching `misp_attribute_uuid` to the attribute's source `uuid`. Supply explicit, unique UUIDs for this purpose.
- Links from tasks to individual attributes inside MISP objects are not restored by the current case importer. Review and recreate those links after import.

### Object references

An object's `references` array describes outgoing relationships. Each entry supplies `relationship_type` and optionally `comment`, plus a target:

| Target | Preferred reference fields | Fallback when the UUID cannot be resolved |
| --- | --- | --- |
| Another imported object | `referenced_object_uuid` | `referenced_object_template_uuid` and `referenced_object_name` |
| An imported attribute, standalone or inside an object | `referenced_type: "attribute"` and `referenced_attribute_uuid` | `referenced_attribute_type`, `referenced_attribute_object_relation` and `referenced_attribute_value` |

References are resolved after objects and attributes have been created. Keep source UUIDs consistent throughout the payload. Targets must be present in the same imported case; unresolved references are skipped. UUIDs avoid ambiguity when several objects or attributes share the same fallback fields.

## Export and import limitations

Import creates a new investigation owned by the importing user's organisation. It is not a full-fidelity restore of every field in a downloaded case.

- Exported task `status`, task `external_references`, and subtask completion state are not restored by this importer. Review the new case's workflow state and external references after import.
- Assignments, original history, connector configuration and attachment files are not restored by this JSON workflow. A case export does not replace a database/filesystem backup; see [Operations and backups]({{< relref "operations" >}}).
- Recurrence currently has an export/import mismatch: downloads emit `recurring_date` as `YYYY-MM-DD`, while the importer parses a non-empty value as `YYYY-MM-DD HH:MM` and requires `recurring_type`. The case creation path does not restore the recurrence schedule. Set both fields to `null` for import, then configure recurrence in the imported case.
- Case import uses `tasks`; a payload containing `tasks_template` is rejected. Use the template importer for templates.
- Batch import processes entries in sequence. An error can leave earlier cases imported. Read the response message and inspect the destination before retrying; an HTTP success status alone does not confirm that every case was created.

## Templates and MISP events

Template JSON goes to template import or `POST /api/importer/template`, which requires Template Editor permission or administrator access. See [Templates]({{< relref "templates" >}}).

For an event on a MISP instance, use **Create case from MISP** or `POST /api/case_from_misp/`. This workflow selects a case template, a MISP instance and an event, with optional content selections. For unsolicited event submissions awaiting triage, use [Incoming alerts]({{< relref "alerts" >}}).
