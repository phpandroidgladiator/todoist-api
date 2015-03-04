# Projects

> A project in Todoist is a JSON object. Typically a project object will look like this:

```shell
{
  "color": 1,
  "collapsed": 0,
  "archived_date": null,
  "indent": 1,
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
color | The color of the project, an index is an array. For example `#bde876`, `#ff8581`, etc.
indent | The indent of the item (a number between 1 and 4 where 1 is top-level).
item_order | Project's order in the project list. Smaller number should be located in the top.
collapsed | If set to 1 the project's sub projects are collapsed. Otherwise they aren't.

## Add a project

> An example of adding a project:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "project_add", "temp_id": "4ff1e388-5ca6-453a-b0e8-662ebf373b6b", "uuid": "32774db9-a1da-4550-8d9d-910372124fa4", "args": {"name": "Project4"}}]'
{ ...
  "SyncStatus": {"32774db9-a1da-4550-8d9d-910372124fa4": "ok"},
  "TempIdMapping": {"4ff1e388-5ca6-453a-b0e8-662ebf373b6b": 128501815},
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> project = api.projects.add('Project4')
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
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands=[{"type": "project_update", "uuid": "1ca42128-d12f-4a66-8413-4d6ff2838fde", "args": {"id": 128501815, "indent": 2}}]'
{ ...
  "SyncStatus": {"1ca42128-d12f-4a66-8413-4d6ff2838fde": "ok"},
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
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands=[{"type": "project_delete", "uuid": "367182ba-125f-4dbb-bff6-c1343fd751e4", "args": {"ids": [128501815]}}]'
{ ...
  "SyncStatus": {"367182ba-125f-4dbb-bff6-c1343fd751e4": {"128501815": "ok"}},
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
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands=[{"type": "project_archive", "uuid": "bbec1a60-2bdd-48ac-a623-c8eb968e1697", "args": {"id": 128501682}}]'
{ ...
  "SyncStatus": {"bbec1a60-2bdd-48ac-a623-c8eb968e1697": "ok},
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
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands=[{"type": "project_unarchive", "uuid": "7d86f042-e098-4fa6-9c1f-a61fe8c39d74", "args": {"id": 128501682}}]'
{ ...
  "SyncStatus": {"7d86f042-e098-4fa6-9c1f-a61fe8c39d74": "ok"},
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
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands=[{"type": "project_update_orders_indents", "uuid": "bf0855a3-0138-4b76-b895-88cad8db9edc", "args": {"ids_to_orders_indents": {"128501470": [42, 1], "128501607": [43, 1]}}}]'
{ ...
  "SyncStatus": {
    "bf0855a3-0138-4b76-b895-88cad8db9edc": {
      "128501470": "ok",
      "128501607": "ok"
    },
  },
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.projects.update_orders_indents({128501470: [42, 1], 128501607: [43, 1]})
>>> api.commit()
```

Update the orders and indents of multiple projects at once.

### Required arguments

Argument | Description
-------- | -----------
ids_to_orders_indents | A dictionary, where a project id is the key, and a list with two elements, the order and the indent, are its value: `project_id: [item_order, indent]`.
