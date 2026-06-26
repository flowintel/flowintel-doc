---
title: "Notifications"
weight: 60
---

Each time you click the bell on a task to notify a user, a notification is sent.

Recurring cases are handled by a background screen. Every day at 2am, the script
creates a notification for each recurring case, for all the users involved.

## Screen

To open the screen, for example to debug it:

```bash
screen -r fcm
```

## Modules

You can notify users through modules, such as email or Matrix. Here is an
[example](https://github.com/flowintel/flowintel/blob/main/app/modules/notify_user/email.py).

If your own module needs parameters, set them in
[config_module.py](https://github.com/flowintel/flowintel/blob/main/conf/config_module.py).
