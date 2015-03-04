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
    -d commands='[{"type": "label_register", "temp_id": "f2f182ed-89fa-4bbb-8a42-ec6f7aa47fd0", "uuid": "ba204343-03a4-41ff-b964-95a102d12b35", "args": {"name": "Label1"}}]'
{ ...
  "SyncStatus": {"ba204343-03a4-41ff-b964-95a102d12b35": "ok"},
  "TempIdMapping": {"f2f182ed-89fa-4bbb-8a42-ec6f7aa47fd0": 1234},
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
    -d commands='[{"type": "label_update", "uuid": "9c9a6e34-2382-4f43-a217-9ab017a83523", "args": {"id": 1234, "color": 3}}]'
{ ...
  "SyncStatus": {"9c9a6e34-2382-4f43-a217-9ab017a83523": "ok"},
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
    -d commands='[{"type": "label_delete", "uuid": "aabaa5e0-b91b-439c-aa83-d1b35a5e9fb3", "args": {"id": 1234}}]'
{ ...
  "SyncStatus": {"aabaa5e0-b91b-439c-aa83-d1b35a5e9fb3": "ok"},
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
