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
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "label_register", "temp_id": "$1412168307389", "timestamp": 1412168307389, "args": {"name": "Label1"}}]'
{"$1411732480502": 1234}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> label = api.label_register('Label1')
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
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "label_update", "timestamp": 1412168745409, "args": {"id": 1234, "color": 3}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> label = api.label_get_by_id(1234)
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
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "label_delete", "timestamp": 1412169068142, "args": {"id": 1234}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> label = api.label_get_by_id(1234)
>>> label.delete()
>>> api.commit()
```

Delete a label.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the bel
