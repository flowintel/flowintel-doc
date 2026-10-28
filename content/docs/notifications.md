---
title: "Notifications and reminders"
---

Notifications help you follow assignments, task activity, review requests and recurring reminders. Open the bell icon or notifications page to inspect them, filter by category/type and read state, and mark entries as read or unread.

## Task and case activity

Use a task's notification action when you need to draw someone's attention to it. Privileged-case workflows also notify approvers and requesters as work moves through review. Notifications do not grant access to a case: normal case and organisation permissions still apply.

## Recurring reminders

Configure a case's recurrence type and date to request a one-time, daily, weekly or monthly reminder. The recurring job considers active, incomplete cases and advances the next reminder date where appropriate.

In the current source, `app/bin/recurring_notification.py` schedules this job for **03:00 each day in the server's local timezone**. Keep the worker running at that time; the schedule is not based on each browser's timezone.

For launcher-based deployments, inspect the worker with:

```bash
screen -ls
screen -r fcm
```

Detach from a screen session with `Ctrl+A`, then `D`. Worker output is also written to `logs/fcm.log`. For independently managed services, inspect your supervisor's logs instead.

## External delivery

Email, Matrix and webhook modules can deliver notifications outside the application. Administrators configure delivery settings using system settings and module configuration as appropriate for the deployment.

The default module configuration includes SMTP host/port, sender credentials, Matrix server/user/room, and webhook URL/enabled/secret settings. The SMTP password setting is currently spelled `SENDER_PASSOWRD` in `conf/config_module.py.default`; use the exact configuration name expected by the application.

Optional IMAP settings support archiving sent email notifications. Test delivery with non-sensitive content before relying on it for operational alerts.

## Incoming alerts are a separate workflow

Outbound notifications tell people about activity in Flowintel. [Incoming alerts]({{< relref "alerts" >}}) accept MISP event payloads from external tools for review and possible case creation.
