# Importer

To import a case and its tasks, JSON is needed.

Here the format:

```json
{
  "title": "Super Case",
  "description": "My super case for the documentation",
  "uuid": "0b1f9a85-0d38-46a1-b9dd-1eeea1608308",
  "deadline": null,
  "recurring_date": null,
  "recurring_type": null,
  "notes": "This case is just boring...",
  "tags": [],
  "clusters": [],
  "tasks": [
    {
      "title": "Prepare a super tea",
      "description": "Keep it as hot as possible",
      "uuid": "ddd271b4-d7f8-4af0-a9b3-46ad52aca1bf",
      "notes": [
        {
          "uuid": "",
          "note": "# Preparation\n- add one sugar\n",
          "task_uuid": "ddd271b4-d7f8-4af0-a9b3-46ad52aca1bf"
        }
      ],
      "url": "",
      "deadline": null,
      "tags": ["PAP:RED"],
      "clusters": []
    }
  ]
}
```
