# API
Here is some example of how to use the API.

## Case
> [!NOTE]
> Full documentation `/api/case/doc`.
#### Create a case
<!-- tabs:start -->
#### **Python**
```Python
headers = {
    "X-API-KEY": "YOUR-API-KEY-HERE",
    "Content-Type": "application/json"
}

data = {
    "title": "Super Case from API"
}

url = "http://127.0.0.1:7006/api/case/create"
r = requests.post(url, json=data, headers=headers)
print(r.text)
```

#### **curl**
```bash
curl -H "Content-Type: application/json"\
     -H "X-API-KEY: YOUR-API-KEY-HERE"\
     -X POST 127.0.0.1:7006/api/case/create\
     -d '{"title": "Super Case from API"}'
```
<!-- tabs:end -->

<!-- ------------------------------------------------------------- -->
#### List cases
<!-- tabs:start -->
#### **Python**
```Python
headers = {
    "X-API-KEY": "YOUR-API-KEY-HERE"
}

url = "http://127.0.0.1:7006/api/case/all"
r = requests.get(url, headers=headers)
print(r.text)
```

#### **curl**
```bash
curl -H "X-API-KEY: YOUR-API-KEY-HERE" -X GET 127.0.0.1:7006/api/case/all
```
<!-- tabs:end -->

<!-- ------------------------------------------------------------- -->
#### Search a case
<!-- tabs:start -->
#### **Python**
```Python
headers = {
    "X-API-KEY": "YOUR-API-KEY-HERE"
}
data = {
    "search": "Case"
}

url = "http://127.0.0.1:7006/api/case/search"
r = requests.post(url, json=data, headers=headers)
print(r.text)
```

#### **curl**
```bash
curl -H "Content-Type: application/json"\
     -H "X-API-KEY: YOUR-API-KEY-HERE"\
     -X POST 127.0.0.1:7006/api/case/search\
     -d '{"search": "Case"}'
```
<!-- tabs:end -->

<!-- ------------------------------------------------------------- -->
#### Delete a case
<!-- tabs:start -->
#### **Python**
```Python
headers = {
    "X-API-KEY": "YOUR-API-KEY-HERE"
}

url = "http://127.0.0.1:7006/api/case/1/delete"
r = requests.get(url, headers=headers)
print(r.text)
```

#### **curl**
```bash
curl -H "X-API-KEY: YOUR-API-KEY-HERE" -X GET 127.0.0.1:7006/api/case/1/delete
```
<!-- tabs:end -->

<!-- ------------------------------------------------------------- -->
#### Complete a case
<!-- tabs:start -->
#### **Python**
```Python
headers = {
    "X-API-KEY": "YOUR-API-KEY-HERE"
}

url = "http://127.0.0.1:7006/api/case/1/complete"
r = requests.get(url, headers=headers)
print(r.text)
```

#### **curl**
```bash
curl -H "X-API-KEY: YOUR-API-KEY-HERE" -X GET 127.0.0.1:7006/api/case/1/complete
```
<!-- tabs:end -->

<!-- ***************************************************************** -->
<!-- ***************************************************************** -->
<!-- ***************************************************************** -->

## Task
> [!NOTE]
> Full documentation `/api/task/doc`.
#### Create a Task
<!-- tabs:start -->
#### **Python**
```Python
headers = {
    "X-API-KEY": "YOUR-API-KEY-HERE",
    "Content-Type": "application/json"
}

data = {
    "title": "First task from API"
}

url = "http://127.0.0.1:7006/api/case/1/create_task"
r = requests.post(url, json=data, headers=headers)
print(r.text)
```

#### **curl**
```bash
curl -H "Content-Type: application/json"\
     -H "X-API-KEY: YOUR-API-KEY-HERE"\
     -X POST 127.0.0.1:7006/api/case/1/create_task\
     -d '{"title": "First task from API"}'
```
<!-- tabs:end -->

<!-- ------------------------------------------------------------- -->
#### List tasks
<!-- tabs:start -->
#### **Python**
```Python
headers = {
    "X-API-KEY": "YOUR-API-KEY-HERE"
}

url = "http://127.0.0.1:7006/api/case/1/tasks"
r = requests.get(url, headers=headers)
print(r.text)
```

#### **curl**
```bash
curl -H "X-API-KEY: YOUR-API-KEY-HERE" -X GET 127.0.0.1:7006/api/case/1/tasks
```
<!-- tabs:end -->

<!-- ------------------------------------------------------------- -->
#### Delete a task
<!-- tabs:start -->
#### **Python**
```Python
headers = {
    "X-API-KEY": "YOUR-API-KEY-HERE"
}

url = "http://127.0.0.1:7006/api/task/1/delete"
r = requests.get(url, headers=headers)
print(r.text)
```

#### **curl**
```bash
curl -H "X-API-KEY: YOUR-API-KEY-HERE" -X GET 127.0.0.1:7006/api/task/1/delete
```
<!-- tabs:end -->

<!-- ------------------------------------------------------------- -->
#### Complete a case
<!-- tabs:start -->
#### **Python**
```Python
headers = {
    "X-API-KEY": "YOUR-API-KEY-HERE"
}

url = "http://127.0.0.1:7006/api/task/1/complete"
r = requests.get(url, headers=headers)
print(r.text)
```

#### **curl**
```bash
curl -H "X-API-KEY: YOUR-API-KEY-HERE" -X GET 127.0.0.1:7006/api/task/1/complete
```
<!-- tabs:end -->

<!-- ***************************************************************** -->
<!-- ***************************************************************** -->
<!-- ***************************************************************** -->

## Admin
> [!NOTE]
> Full documentation `/api/admin/doc`.
#### Add user
<!-- tabs:start -->
#### **Python**
```Python
headers = {
    "X-API-KEY": "YOUR-API-KEY-HERE",
    "Content-Type": "application/json"
}

data = {
    "first_name": "John",
    "last_name": "Doe",
    "email": "j.d@j.admin",
    "password": "Password1234",
    "role": 1
}

url = "http://127.0.0.1:7006/api/admin/add_user"
r = requests.post(url, json=data, headers=headers)
print(r.text)
```

#### **curl**
```bash
curl -H "Content-Type: application/json"\
     -H "X-API-KEY: YOUR-API-KEY-HERE"\
     -X POST 127.0.0.1:7006/api/admin/add_user\
     -d '{"first_name": "John", "last_name": "Doe", "email": "j.d@j.admin", "password": "Password1234", "role": 1}'
```
<!-- tabs:end -->

<!-- ------------------------------------------------------------- -->
#### List roles
<!-- tabs:start -->
#### **Python**
```Python
headers = {
    "X-API-KEY": "YOUR-API-KEY-HERE"
}

url = "http://127.0.0.1:7006/api/admin/roles"
r = requests.get(url, headers=headers)
print(r.text)
```

#### **curl**
```bash
curl -H "X-API-KEY: YOUR-API-KEY-HERE" -X GET 127.0.0.1:7006/api/admin/roles
```
<!-- tabs:end -->

## Templating
> [!NOTE]
> Full documentation `/api/template/doc`.

#### Create a case
<!-- tabs:start -->
#### **Python**
```Python
headers = {
    "X-API-KEY": "YOUR-API-KEY-HERE",
    "Content-Type": "application/json"
}

data = {
    "title": "Super Template Case from API"
}

url = "http://127.0.0.1:7006/api/template/create_case"
r = requests.post(url, json=data, headers=headers)
print(r.text)
```

#### **curl**
```bash
curl -H "Content-Type: application/json"\
     -H "X-API-KEY: YOUR-API-KEY-HERE"\
     -X POST 127.0.0.1:7006/api/template/create_case\
     -d '{"title": "Super Template Case from API"}'
```
<!-- tabs:end -->

<!-- ------------------------------------------------------------- -->
#### Create a case from a template
<!-- tabs:start -->
#### **Python**
```Python
# A template need to be created before

headers = {
    "X-API-KEY": "YOUR-API-KEY-HERE",
    "Content-Type": "application/json"
}

data = {
    "title": "Super Case from Template API"
}

url = "http://127.0.0.1:7006/api/template/create_case_from_template/1"
r = requests.post(url, json=data, headers=headers)
print(r.text)
```

#### **curl**
```bash
curl -H "Content-Type: application/json"\
     -H "X-API-KEY: YOUR-API-KEY-HERE"\
     -X POST 127.0.0.1:7006/api/template/create_case_from_template/1\
     -d '{"title": "Super Case from Template API"}'
```
<!-- tabs:end -->

<!-- ***************************************************************** -->
<!-- ***************************************************************** -->
<!-- ***************************************************************** -->

## Importer
Json dict to import and used for this example
```python
{
    "uuid": "be8ebf37-6a11-43d1-a4d2-cce097b2bddd",
    "title": "Case test",
    "description": "My super case for the documentation",
    "recurring_type": None,
    "notes": "This case is just boring...\nYes it is !",
    "deadline": None,
    "recurring_date": None,
    "tags": [],
    "clusters": [],
    "tasks": [
        {
            "uuid": "5a1f4902-9d54-476f-a4f8-2a93cbe20597",
            "title": "Prepare a super tea",
            "description": "Keep it as hot as possible",
            "url": "",
            "notes": [
                {
                    "uuid": "864fa9c7-b4d6-4e9b-80e7-0cc07c894e60",
                    "note": "# Preparation\n- add one sugar\n",
                    "task_uuid": "5a1f4902-9d54-476f-a4f8-2a93cbe20597"
                }
            ],
            "deadline": None,
            "tags": [
                "PAP:RED"
            ],
            "clusters": []
        }
    ]
}
```
<!-- tabs:start -->
#### **Python**
```Python
headers = {
    "X-API-KEY": "YOUR-API-KEY-HERE",
    "Content-Type": "application/json"
}

data = ... # Use the dict above.

url = "http://127.0.0.1:7006/api/importer/"
r = requests.post(url, json=data, headers=headers)
print(r.text)
```

#### **curl**
Save the dict above in a file named `CaseTest.json`.
```bash
curl -H "Content-Type: application/json"\
     -H "X-API-KEY: YOUR-API-KEY-HERE"\
     -X POST 127.0.0.1:7006/api/importer/\
     -d @CaseTest.json
```
<!-- tabs:end -->