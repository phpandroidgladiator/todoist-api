# Projects

> A project in Todoist is a JSON object. Typically a project object will look like this:

```shell
{
  "last_updated": "",
  "color": 1,
  "collapsed": 0,
  "archived_date": null,
  "indent": 1,
  "cache_count": 0,
  "is_deleted": 0,
  "id": 128501470,
  "user_id": 1855589,
  "name": "Project1",
  "archived_timestamp": 0,
  "item_order": 36,
  "shared": false,
  "is_archived": 0
}
```

```python
{
  'shared': False,
  'last_updated': '',
  'name': 'Project1',
  'user_id': 1855589,
  'color': 1,
  'is_deleted': 0,
  'collapsed': 0,
  'archived_date': None,
  'item_order': 36,
  'indent': 1,
  'is_archived': 0,
  'archived_timestamp': 0,
  'cache_count': 0,
  'id': 128501470
}
```

A project in Todoist is a JSON object. Typically a project will have the following properties:

### Properties

Property | Description
-------- | -----------
id | The unique id of the project.
user_id | The unique id of the user that owns the project.
name | The name of the project. If the name starts with a star `*` then the name is a group. In this example, `* Life` is a group:
cache_count | How many items does the project hold? (Note: This attribute is deprecated and is not updated on any API calls. If you want to get the number of items in the project, you should fetch all the project's items.)
color | The color of the project, an index is an array. For example `#bde876`, `#ff8581`, etc.
indent | The indent of the item (a number between 1 and 4 where 1 is top-level).
item_order | Project's order in the project list. Smaller number should be located in the top.
collapsed | If set to 1 the project's sub projects are collapsed. Otherwise they aren't.
last_updated | A timestamp when the project was last updated (updated means, name changed, task added, task completed etc.)

## Add a project

> An example of adding a project:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "project_add", "temp_id": "$1411654292.1", "timestamp": "1411654292.1", "args": {"name": "Project4"}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1411654292.1"}],
  "TempIdMapping": {'$1411654292.1': 128501815},
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> project = api.project_add('Project4')
>>> api.commit()
```

Add a new project.

### Required arguments

Argument | Description
-------- | -----------
name | The name of the new project.

### Optional arguments

Argument | Description
-------- | -----------
color | The color of the new project.
indent | The indent of the item (a number between `1` and `4`, where `1` is top-level).
order | The order of the new project.

## Update a project

> An example of updating a project:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d commands=[{"type": "project_update", "timestamp": "1411654327.1", "args": {"id": 128501815, "indent": 2}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1411654327.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> project = api.projects.get_by_id(128501815)
>>> project.update(indent=2)
>>> api.commit()
```

Update an existing project.

### Required parameters

Argument | Description
-------- | -----------
id | The id of the project to update.

### Optional arguments

Argument | Description
-------- | -----------
name | New name of the project.
color | New color of the project.
indent | The indent of the item (a number between `1` and `4`, where `1` is top-level).
order | The order of the project.
collapsed | `1` if the project should be collapsed, `0` if it should not be collapsed.

## Delete projects

> An example of deleting a project:

```shell

$ curl https://todoist.com/API/v6/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d commands=[{"type": "project_delete", "timestamp": "1411654352.1", "args": {"ids": [128501815]}}]'
{ ...
  "SyncStatus": [{"status": [{"128501815": "ok"}], "timestamp": "1411654352.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> project = api.projects.get_by_id(128501815)
>>> project.delete()
>>> api.commit()
```

Delete an existing project.

### Required arguments

Argument | Description
-------- | -----------
ids | List of the ids of the projects to delete.


## Archive a project

> An example of archiving a project:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d commands=[{"type": "project_archive", "timestamp": "1411654438.1", "args": {"id": 128501682}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1411654438.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> project = api.projects.get_by_id(128501682)
>>> project.archive()
>>> api.commit()
```

Archive project and its children. Only available for Todoist Premium users.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the project to archive.

## Unarchive a project

> An example of unarchiving a project:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d commands=[{"type": "project_unarchive", "timestamp": "1411654459.1", "args": {"id": 128501682}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1411654459.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> project = api.projects.get_by_id(128501815)
>>> project.unarchive()
>>> api.commit()
```

Unarchive project and its children. Only available for Todoist Premium users.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the project to unarchive.

## Update multiple orders/indents

> An example of updating the orders and indents of multiple projects at once:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d commands=[{"type": "project_update_orders_indents", "timestamp": "1411654466.1", "args": {"ids_to_orders_indents": {"128501470": [42, 1], "128501607": [43, 1]}}}]'
{ ...
  "SyncStatus": [
    { "status":
      [ {"128501470": "ok"},
        {"128501607": "ok"} ],
      "timestamp": "1411654459.1" },
  ],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.project_update_orders_indents({128501470: [42, 1], 128501607: [43, 1]})
>>> api.commit()
```

Update the orders and indents of multiple projects at once.

### Required arguments

Argument | Description
-------- | -----------
ids_to_orders_indents | A dictionary, where a project id is the key, and a list with two elements, the order and the indent, are its value: `project_id: [item_order, indent]`.
