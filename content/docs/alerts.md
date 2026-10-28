---
title: "Incoming alerts and triage"
---

Incoming alerts let external tools submit MISP event data for review before an analyst creates a case. This is separate from outbound email, Matrix and webhook notifications.

## Send an alert

Use `POST /api/alerts/ingest` with a JSON MISP event, either wrapped as `{"Event": {...}}` or supplied as the event object itself. Include event information or a UUID and attribute and/or object content. The endpoint limits request bodies to **512 KiB**.

Authentication uses a configured **connector instance key**, including a supported user-specific connector key. This differs from the ordinary user API key used by case-management endpoints. The endpoint accepts `X-API-KEY`, `X-FLOWINTEL-ALERT-KEY` or a Bearer token.

Save this example as `alert.json`:

```json
{
  "Event": {
    "uuid": "8593027b-1d43-4ef0-9ef0-cd9392b1f614",
    "info": "Domain requiring triage",
    "Attribute": [
      {"type": "domain", "value": "example.org", "to_ids": false}
    ]
  }
}
```

With `FLOWINTEL_URL` and `FLOWINTEL_ALERT_KEY` set in your shell:

```bash
curl --fail-with-body \
  -H "X-FLOWINTEL-ALERT-KEY: $FLOWINTEL_ALERT_KEY" \
  -H 'Content-Type: application/json' \
  --data-binary @alert.json \
  "$FLOWINTEL_URL/api/alerts/ingest"
```

A newly created alert returns HTTP **201**; an update to an existing alert returns **200**. The response includes `alert_id`, `alert_uuid`, `created`, `review_status` and `occurrence_count`. Inspect these fields instead of assuming every submission creates a new alert.

## Review and create a case

Open the incoming alert review interface, inspect the event and its indicators, and decide whether investigation is warranted. Review statuses include `new`, `reviewing`, `case_created`, `false_positive` and `dismissed`.

Create a case from an actionable alert using the available case-creation workflow. Keep the distinction between reading an alert and deciding its review outcome: read state alone does not mean it has been investigated.

## Inspect the accepted format

`GET /api/alerts/schema` returns the format description and an example. This schema endpoint uses an ordinary authenticated **user API key**, whereas ingestion uses the connector instance key.

For request errors, check the JSON structure, body size and connector credentials. HTTP 400 indicates an invalid payload, 403 an unrecognised connector key, and 413 an oversized request.
