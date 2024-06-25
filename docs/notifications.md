# Notifications

Each time the bell is clicked on a task to notify a user a notification is send.

Regarding recurring case a screen is used. Every day at 2am the script create a notification for each recurring case for all users concern.

## Screen

To access the screen and eventually debug:

```bash
screen -r fcm
```

## Modules

It's possible to notify user using modules like email, matrix... Here an [example](https://github.com/flowintel/flowintel/blob/main/app/modules/notify_user/email.py).

If some parameters is needed to your own module, use [config_module.py](https://github.com/flowin.tel/flowintel/blob/main/conf/config_module.py)
