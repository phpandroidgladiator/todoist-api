# Items

> A item in Todoist is a JSON object. The dates will be in UTC timezone. Typically a item object will look like:

```shell
{
  "due_date": null,
  "day_order": -1,
  "assigned_by_uid": 1855589,
  "is_archived": 0,
  "labels": [],
  "sync_id": null,
  "in_history": 0,
  "has_notifications": 0,
  "indent": 1,
  "checked": 0,
  "date_added": "Fri 26 Sep 2014 08:25:05 +0000",
  "id": 33511505,
  "content": "Task1",
  "user_id": 1855589,
  "due_date_utc": null,
  "children": null,
  "priority": 1,
  "item_order": 1,
  "is_deleted": 0,
  "responsible_uid": null,
  "project_id": 128501470,
  "collapsed": 0,
  "date_string": ""
}
```

```python
{
  'is_archived': 0,
  'labels': [],
  'sync_id': None,
  'in_history': 0,
  'indent': 1,
  'checked': 0,
  'children': None,
  'priority': 1,
  'user_id': 1855589,
  'id': 33511505,
  'content': 'Task1',
  'item_order': 1,
  'project_id': 128501470,
  'date_string': '',
  'due_date': None,
  'day_order': -1,
  'assigned_by_uid': 1855589,
  'collapsed': 0,
  'has_notifications': 0,
  'date_added': 'Fri 26 Sep 2014 08:25:05 +0000',
  'is_deleted': 0,
  'due_date_utc': None,
  'responsible_uid': None
}
```

### Properties

Property | Description
-------- | -----------
id | Unique task id.
user_id | The owner of the task.
content | The text of the task.
project_id | The id of the project to add the task to.
date_string | The date of the task, added in free form text, for example it can be `every day @ 10`. Look at our reference to see [which formats are supported](https://todoist.com/Help/timeInsert).
due_date_utc | Should be formatted as `YYYY-MM-DDTHH:MM`, example: `2012-3-24T23:59`. Value of `due_date_utc` must be in UTC. If you want to pass in due dates, note that `date_string` is required, while `due_date_utc` (`due_date`) can be omitted. If date_string is provided, it will be parsed as local timestamp, and converted to UTC internally, according to the user's profile settings.
due_date | Alternative (backward compatible, not recommended) way of providing the item due date. Format is the same as for `due_date_utc`, must be in UTC for due dates containing time fragments, and in the format `YYYY-MM-DDT23:59:59` (hardcoded timestamp) for whole-day entries.
js_date | If `js_date` is set to `1` dates will be formated as `new Date("Sun Apr 29 2007 23:59:59")`, otherwise they will be formatted as `"Sun Apr 29 2007 23:59:59"`.
in_history | If set to `1`, the task is marked completed.
collapsed | If set to `1` the task's sub tasks are collapsed. Otherwise they aren't.
indent | The indent of the item (a number between `1` and `4`, where `1` is top-level).
priority | The priority of the task (a number between `1` and `4`, `4` for very urgent and `1` for natural).
indent | The indent of the item (a number between `1` and `4`, where `1` is top-level).
item_order | The order of the task.
children | The tasks child tasks (a list of task ids such as `[13134,232345]`).
labels | The tasks labels (a list of label ids such as `[2324,2525]`).
assigned_by_uid | The id of user who assigns current task. Makes sense for shared projects only. Accepts `0` or any user id from the list of project collaborators. If this value is unset or invalid, it will automatically be set up by your uid.
responsible_uid | The id of user who is responsible for accomplishing the current task. Makes sense for shared projects only. Accepts 0 or any user id from the list of project collaborators. If this value is unset or invalid, it will automatically be set up by null.

## Add an item

> An example of adding a task:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "item_add", "temp_id": "$1411732480.1", "timestamp": "1411732480.1", "args": {"content": "Task1", "project_id": 128501470}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1411732480.1"}],
  "TempIdMapping": {"$1411732480.1": 33548400},
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> item = api.items.add('Task1', 128501470)
>>> api.commit()
```

Add a new task to a project.

### Required arguments

Argument | Description
-------- | -----------
content | The text of the task.
project_id | The id of the project to add the task to.

### Optional arguments

Argument | Description
-------- | -----------
priority | The priority of the task (a number between `1` and `4`, `4` for very urgent and `1` for natural).
indent | The indent of the item (a number between `1` and `4`, where `1` is top-level).
date_string | The date of the task, added in free form text, for example it can be `every day @ 10`. Look at our reference to see [which formats are supported](https://todoist.com/Help/timeInsert).
due_date_utc | Should be formatted as `YYYY-MM-DDTHH:MM`, example: `2012-3-24T23:59`. Value of `due_date_utc` must be in UTC. If you want to pass in due dates, note that `date_string` is required, while `due_date_utc` (`due_date`) can be omitted. If date_string is provided, it will be parsed as local timestamp, and converted to UTC internally, according to the user's profile settings.
due_date | Alternative (backward compatible, not recommended) way of providing the item due date. Format is the same as for `due_date_utc`, must be in UTC for due dates containing time fragments, and in the format `YYYY-MM-DDT23:59:59` (hardcoded timestamp) for whole-day entries.
js_date | If `js_date` is set to `1` dates will be formated as `new Date("Sun Apr 29 2007 23:59:59")`, otherwise they will be formatted as `"Sun Apr 29 2007 23:59:59"`.
item_order | The order of the task.
children | The tasks child tasks (a list of task ids such as `[13134,232345]`)
labels | The tasks labels (a list of label ids such as `[2324,2525]`)
assigned_by_uid | The id of user who assigns current task. Makes sense for shared projects only. Accepts `0` or any user id from the list of project collaborators. If this value is unset or invalid, it will automatically be set up by your uid.
responsible_uid | The id of user who is responsible for accomplishing the current task. Makes sense for shared projects only. Accepts `0` or any user id from the list of project collaborators. If this value is unset or invalid, it will automatically be set up by null.
note | Add a note directly to the task, note is a string of the content.

## Update an item

> An example of updating a task:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "item_update", "timestamp": "1411984521.1", "args": {"id": 33548400, "priority": 2}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1411984521.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> item = api.items.get_by_id(33548400)
>>> item.update(priority=2)
>>> api.commit()
```

Update an existing task.

### Required arguments
Argument | Description
-------- | -----------
id | The id of the item to update.

### Optional arguments

Argument | Description
-------- | -----------
content | The text of the task.
date_string | The date of the task, added in free form text, for example it can be `every day @ 10`. Look at our reference to see [which formats are supported](https://todoist.com/Help/timeInsert).
priority | The priority of the task (a number between `1` and `4`, `4` for very urgent and `1` for natural).
indent | The indent of the item (a number between `1` and `4`, where `1` is top-level).
item_order | The order of the task.
collapsed | `1` if the item should be collapsed, `0` if it should not be collapsed.
children | The tasks child tasks (a list of task ids such as `[13134,232345]`)
labels | The tasks labels (a list of label ids such as `[2324,2525]`)
js_date | If js_date is set to 1 dates will be formated as `new Date("Sun Apr 29 2007 23:59:59")`, otherwise they will be formatted as `"Sun Apr 29 2007 23:59:59"`.
assigned_by_uid | The id of user who assigns current task. Makes sense for shared projects only. Accepts `0` or any user id from the list of project collaborators. If this value is unset or invalid, it will automatically be set up by your uid.
responsible_uid | The id of user who is responsible for accomplishing the current task. Makes sense for shared projects only. Accepts `0` or any user id from the list of project collaborators. If this value is unset or invalid, it will automatically be set up by null.

## Delete items

> An example of deleting a task:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "item_delete", "timestamp": "1411984533.1", "args": {"ids": [33548400]}}]'
{ ...
  "SyncStatus": [{"status": [{"33548400": "ok"}], "timestamp": "1411984533.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> item = api.items.get_by_id(33548400)
>>> item.delete()
>>> api.commit()
```

Delete an existing task.

### Required arguments

Argument | Description
-------- | -----------
ids | List of the ids of the items to delete.

## Move an item

> An example of moving of a task from one project to another project:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "item_move", "timestamp": "1411997433.1", "args": {"project_items": {"128501470": [33548400]}, "to_project": 128501607}}]'
{ ...
  "SyncStatus": [{"status": [{"33548400": "ok"}], "timestamp": "1411997433.1"}],
  ... }

```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.items.get_by_id(33548400)
>>> item.move(128501607)
>>> api.commit()
```

Move a task from one project to another project.

### Required arguments
Argument | Description
-------- | -----------
project_items | A JSON mapping telling Todoist where the items are currently found. From project ids to item ids, could be like this `{"1523":["9637423"]}`, where `1523` is project id and `9637423` is item id.
to_project | A project_id that the items should be moved to. Could be `1245`.

## Complete items

> An example of completing a task:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "item_complete", "timestamp": "1412001010.1", "args": {"project_id": 128501470, "ids": [33548400]}}]'
{ ...
  "SyncStatus": [{"status": [{"33548400": "ok"}], "timestamp": "1412001010.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.items.get_by_id(33548400)
>>> item.complete()
>>> api.commit()
```

Complete tasks and move them to history.

### Required arguments

Argument | Description
-------- | -----------
project_id | The id of the project which the items are part of.
ids | A JSON list of ids to complete.

### Optional arguments

Argument | Description
-------- | -----------
in_history | If these tasks should be moved to history, default is `1`. Setting it to `0` will not move it to history. Useful when checking off sub tasks.

## Uncomplete items

> An example of uncompleting a task:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "item_uncomplete", "timestamp": "1412001918.1", "args": {"project_id": 128501470, "ids": [33548400]}}]'
{ ...
  "SyncStatus": [{"status": [{"33548400": "ok"}], "timestamp": "1412001918.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.items.get_by_id(33548400)
>>> item.uncomplete()
>>> api.commit()
```

Uncomplete tasks and move them to the active projects.

### Required arguments

Argument | Description
-------- | -----------
project_id | The id of the project to which the items will be moved to.
ids | A JSON list of ids to uncomplete.

### Optional arguments

Argument | Description
-------- | -----------
update_item_orders | If this is set to `0` the item orders should not be updated, while by default this is set `1` and they are updated.

## Restore multiple metadata

> An example of restoring metadata about uncompleted items:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "item_uncomplete_update_meta", "timestamp": "1412061611.1", "args": {"project_id": 128501470, "ids_to_metas": {"33548400": [0, 0, 1]}}}]'
{ ...
  "SyncStatus": [{"status": [{"33548400": "ok"}], "timestamp": "1412061611.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.items.uncomplete_update_meta(128501470, {33548400: [0, 0, 1]})
>>> api.commit()
```

Restore metadata about uncompleted items.

### Required arguments

Argument | Description
-------- | -----------
project_id | The id of the project to which the items belong to.
ids_to_metas | A dictionary, where item id is the key, and its value is a list of three elements, whether the item is in history, whether it is checked, and its order: `item_id: [in_history, checked, item_order]`

## Complete a recurring task

> An example of completing a recurring task:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "item_update_date_complete", "timestamp": "1412089326.1", "args": {"id": 33548400, "new_data_utc": "2014-10-30T23:59", "date_string": "every day", "is_forward": 1}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1412089326.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.items.update_date_complete(33548400, '2014-10-30T23:59', 'every day', 1)
>>> api.commit()
```

Complete a recurring task, and the reason why this is a special case is because we need to mark a recurring completion (and using `item_update` won't do this).

### Required arguments

Argument | Description
-------- | -----------
id | The id of the item to update.
new_date_utc | Should be formatted as `YYYY-MM-DDTHH:MM`.
date_string | The date of the task, added in free form text, for example it can be `every day @ 10`. Look at our reference to see [which formats are supported](https://todoist.com/Help/timeInsert).
is_forward | Indicates if it's a complete `1` or uncomplete `0`.

## Update multiple orders/indents

> An example of updating the orders and indents of multiple items at once:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "item_update_orders_indents", "timestamp": "1412089328.1", "args": {"ids_to_orders_indents": {"33548400": [1, 1]}}}]'
{ ...
  "SyncStatus": [{"status": [{"33548400": "ok"}], "timestamp": "1412089328.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.items.update_orders_indents({33548400: [1, 1]})
>>> api.commit()
```

Update the orders and indents of multiple items at once.

### Required arguments

Argument | Description
-------- | -----------
ids_to_orders_indents | A dictionary, where an item id is the key, and a list with two elements, the order and the indent, are its value: `item_id: [item_order, indent]`.

## Update day orders

> An example of updating the day orders of multiple items at once:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "item_update_day_orders", "timestamp": "1412089509.1", "args": {"ids_to_orders": {"33548400": 1}}}]'
{ ...
  "SyncStatus": [{"status": [{"33548400": "ok"}], "timestamp": "1412089509.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.items.update_day_orders({33548400: 1})
>>> api.commit()
```

Update the day orders of multiple items at once.

### Required arguments

Argument | Description
-------- | -----------
ids_to_orders | A dictionary, where an item id is the key, and the day order its value: `item_id: day_order`.
