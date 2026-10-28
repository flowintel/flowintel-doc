---
title: "Administration and access"
---

Start by creating organisations, user accounts and roles. Review access using a normal analyst account as well as an administrator account, so you can confirm what your team will actually see.

## Organisations and users

Organisations group users and participate in cases. Global administrators manage the instance; organisation administrators manage users within their permitted organisation scope.

Create users from the administration interface and assign an organisation and role. The built-in base roles include **Admin**, **Editor** and **Read Only**. Additional permissions control specialised operations.

| Permission | Typical responsibility |
|---|---|
| Template Editor | Maintain reusable case, task and note templates. |
| MISP Editor | Work with MISP features and connector operations. |
| Importer | Import cases. |
| Audit Viewer | Inspect audit logs. |
| Case Admin / Queue Admin | Approve work within the privileged-case workflow, subject to case access. |
| Queuer | Propose work for review. |

Permissions do not replace case visibility and organisation checks. Grant only the capabilities needed for the user's work.

## Private and privileged cases

**Private** controls visibility: assigned organisations and administrators can access a private case. **Privileged** controls the task approval workflow. These settings serve different purposes and can be combined.

A typical privileged task moves through:

```text
Requested → Approved → Ongoing → Request Review → Finished
```

An approver can reject a proposal or return work for further action. Editing is restricted while tasks await approval or review. Admin, Case Admin and Queue Admin permissions participate in approval; marking a case privileged is restricted to Admin or Case Admin.

The workflow provides review stages, but it should not be presented as a strict technical guarantee that two different people approve every task: approvers can approve their own requests. Apply your team's separation-of-duties procedure alongside the application settings.

Administrators can enforce privileged cases and configure automatic approver assignment through `PRIVILEGED_CASE_ADD_ADMIN_ON_TASK_REQUEST`. Task status identifiers must match the configured review statuses. Organisation view restrictions can further limit non-admin visibility.

## Account setup and recovery

Users manage their profile and API credentials from their account settings. Keep API keys private and use the permissions of a dedicated automation account for scripts.

For initial provisioning or recovery without an existing API key, run the application repository's account tool after the database schema and migrations are ready:

```bash
env/bin/python bin/create_user.py --email admin@example.com \
  --first-name Site --last-name Admin --role Admin
```

The tool prompts for a password. Add `--env production` for a production deployment, and `--org-id ID` to use an existing organisation. Without an organisation ID it creates a personal organisation. Existing users are not overwritten. Run `--help` for the options supported by your checkout.

## Single sign-on

The application supports optional Microsoft Entra ID, Keycloak and SimpleSAML integrations. Configure the provider and matching Flowintel settings before enabling SSO. Callback URLs and user provisioning must match your deployment. Follow the authentication sections of the [application user manual](https://github.com/flowintel/flowintel/blob/main/doc/user-manual.md) and `conf/config.py.default` for provider-specific configuration.

## System settings and audit

System settings cover application identity, branding, privacy notices, logging, MISP behaviour, template repositories and notification delivery. GPG report signing requires a configured signing identity and keys on the server.

Use case history for an investigation's changes and the audit log for application activity. Decide how logs are retained and included in backups. See [Operations and backups]({{< relref "operations" >}}).
