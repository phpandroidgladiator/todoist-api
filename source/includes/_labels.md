# Labels

> A label in Todoist is a JSON object. Typically a label object will look like:

```shell
{
  "is_deleted": 0,
  "uid": 1,
  "color": 7,
  "id": 1234,
  "name": "Label1"
}
```

```python
{
  'color': 7,
  'is_deleted': 0,
  'uid': 1,
  'name': 'Label1',
  'id': 1234
}
```

### Properties

Property | Description
-------- | -----------
id | The id of the label.
name| The name of the label.
color | The color of the label.

## Register a label

> An example of registering a label:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "label_register", "temp_id": "$1412168307.1", "timestamp": "1412168307.1", "args": {"name": "Label1"}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1412168307.1"}],
  "TempIdMapping": {"$1412168307.1": 1234},
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> label = api.labels.register('Label1')
>>> api.commit()
```

Register a label.

### Required arguments

Argument | Description
-------- | -----------
name | The name of the label.

### Optional arguments

Argument | Description
-------- | -----------
color | The color of the label.

## Update a label

> An example of updating a label:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "label_update", "timestamp": "1412168745.1", "args": {"id": 1234, "color": 3}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1412168745.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> label = api.labels.get_by_id(1234)
>>> label.update(color=3)
>>> api.commit()
```

Update a registered label.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the label.

### Optional arguments

Argument | Description
-------- | -----------
name | The name of the label.
color | The color of the label.

## Delete a label

> An example of deleting a label:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "label_delete", "timestamp": "1412169068.1", "args": {"id": 1234}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1412169068.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> label = api.labels.get_by_id(1234)
>>> label.delete()
>>> api.commit()
```

Delete a label.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the bel
