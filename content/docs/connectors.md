---
title: "Connectors"
weight: 20
---

A connector represents the tool you want to connect to. It holds instances,
which contain the information needed to connect to and use that tool.

An instance can be used by several cases. In each case, an identifier is either
set manually or filled in by the module that uses the instance.

![Flowintel connectors](/images/flowintel_connectors.png)

## Modules

A module is a script that takes Flowintel data and sends it to another tool in a
compatible format.

### Create your own module

> [!TIP]
> If your module needs parameters that the user has to fill in, define them in
> `/conf/config_module.py`.

You can find an example [here](https://github.com/flowintel/flowintel/blob/main/app/modules/send_to/misp_event.py).

At a minimum, you need to set these two values:

```python
module_config = {
    "connector": "",    # type of connector to use, can be removed
    "case_task": ""     # 'case' or 'task'
}

def handler(instance, case, user):
    """
    instance: Info about the connector like name, api_key...
    case: Dictionary with all case info, including all its tasks
    user: Info on the user who uses the module
    """
    # Write your code here
    # ...
```

Save your module in the directory that matches its type:

`flowintel/app/modules`
