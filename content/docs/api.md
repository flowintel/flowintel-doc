---
title: "REST API"
---

The API supports case management, templates, integrations and administration. Open **`/api/` on your Flowintel instance** for the combined Swagger interface. The OpenAPI document is available at `/api/swagger.json`. Older per-service `/api/case/doc`-style links are not the current documentation entry point.

## Authentication

Most endpoints require a user's API key in the `X-API-KEY` header. The key acts with that user's role, specialised permissions and case access. Obtain your key from your account settings and keep it out of source code and logs.

The examples below assume these shell variables are set:

```bash
export FLOWINTEL_URL='http://127.0.0.1:7006'
# Set FLOWINTEL_API_KEY to your user API key in your local environment.
```

Use the HTTPS address of your deployment for remote requests. Alert ingestion is an exception: it authenticates with a connector key, described under [Incoming alerts]({{< relref "alerts" >}}).

## Case

Create a case:

```bash
curl --fail-with-body \
  -H "X-API-KEY: $FLOWINTEL_API_KEY" \
  -H 'Content-Type: application/json' \
  --data '{"title":"Domain investigation","description":"Triage the report"}' \
  "$FLOWINTEL_URL/api/case/create"
```

A successful creation returns HTTP 201 with a `case_id`. Use that value for subsequent requests.

| Method | Path | Purpose |
|---|---|---|
| GET | `/api/case/all` | List cases available to the caller. |
| GET | `/api/case/{case_id}` | Retrieve a case. |
| POST | `/api/case/create` | Create a case. |
| POST | `/api/case/{case_id}/edit` | Update case fields. |
| GET | `/api/case/{case_id}/tasks` | List tasks in a case. |
| GET | `/api/case/{case_id}/history` | Read case history. |

Creation supports contextual tags, clusters, custom tags, deadlines, `is_private` and `privileged_case`. Setting the privileged flag requires the corresponding administrative permission. Consult Swagger for field details and the endpoints available in your deployed version.

## Task

Replace `123` with the returned case ID:

```bash
curl --fail-with-body \
  -H "X-API-KEY: $FLOWINTEL_API_KEY" \
  -H 'Content-Type: application/json' \
  --data '{"title":"Triage the report","description":"Record the evidence"}' \
  "$FLOWINTEL_URL/api/case/123/create_task"
```

Success returns HTTP 201 and a `task_id`. Task creation is under the **case** namespace; subsequent operations use `/api/task/{task_id}` and its subroutes for notes, assignment, files, subtasks and status changes.

Use `/api/task/list_status` to discover task statuses rather than hard-coding IDs from another instance. Privileged-case review rules also apply to API operations.

## Python example

This example creates a case and then a task, checks HTTP errors and uses bounded request timeouts. It requires the `requests` package.

```python
import os
import requests

base = os.environ["FLOWINTEL_URL"].rstrip("/")
with requests.Session() as session:
    session.headers["X-API-KEY"] = os.environ["FLOWINTEL_API_KEY"]
    response = session.post(
        f"{base}/api/case/create",
        json={"title": "Domain investigation from Python"},
        timeout=30,
    )
    response.raise_for_status()
    case_id = response.json()["case_id"]

    response = session.post(
        f"{base}/api/case/{case_id}/create_task",
        json={"title": "Review the evidence"},
        timeout=30,
    )
    response.raise_for_status()
    print(response.json())
```

Choose a unique title when rerunning the example. If a write times out, check whether it succeeded before retrying; creating a case and creating its task are separate operations.

## Admin

`/api/admin` exposes user, organisation and role management. Examples include `GET /api/admin/users`, `GET /api/admin/orgs` and `GET /api/admin/roles`. Permissions vary by operation; user management also checks organisation-administrator scope where supported.

## Templating

`/api/templating` manages case and task templates, their context and reusable work items. Template maintenance requires Template Editor permission or administrator access. For exchanging exported templates, use `POST /api/importer/template`.

## Analyser

Start a session with `POST /api/analyzer/misp-modules/query` using a JSON body containing:

- `query`: list of values to analyse.
- `input`: one MISP attribute type.
- `modules`: list of module names compatible with that input.

Use the returned session identifier with `GET /api/analyzer/misp-modules/status/{session_id}` and then `GET /api/analyzer/misp-modules/result/{session_id}`. Module execution is asynchronous; inspect status and result messages rather than treating the initial response as the final analysis.

## Importer

`POST /api/importer/case` accepts Flowintel case JSON; `POST /api/importer/template` accepts template JSON. See [Import and export]({{< relref "importer" >}}) before constructing payloads. `POST /api/case_from_misp/` creates a case from a configured MISP instance/event and template.

## Other services

| Prefix | Purpose |
|---|---|
| `/api/connectors` | Connector and instance management |
| `/api/custom_tags` | Local tags |
| `/api/my_assignment` | Assigned work |
| `/api/calendar` | Calendar operations |
| `/api/alerts` | Incoming alert schema and ingestion |

MISP sync settings and conflict resolution are exposed under `/api/case/{case_id}/connectors/{link_id}/…`. The settings response lists supported intervals, conflict strategies and resolutions.

## Errors and automation

Read both the HTTP status and response body. Typical outcomes include 400 for invalid input, 403 for authentication/permission failures, and 404 for unavailable resources. Some legacy operations report errors in a JSON `message` even when the HTTP response is successful, particularly imports; validate the operation's result too.

Some existing routes mutate data through GET requests. Check the documented method and operation before calling a route; do not indiscriminately crawl authenticated API URLs. Limit retries of writes and verify the existing state after a timeout.
