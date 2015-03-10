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
    -d commands='[{"type": "item_add", "temp_id": "43f7ed23-a038-46b5-b2c9-4abda9097ffa", "uuid": "997d4b43-55f1-48a9-9e66-de5785dfd69b", "args": {"content": "Task1", "project_id": 128501470}}]'
{ ...
  "SyncStatus": {"997d4b43-55f1-48a9-9e66-de5785dfd69b": "ok"},
  "TempIdMapping": {"43f7ed23-a038-46b5-b2c9-4abda9097ffa": 33548400},
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
    -d commands='[{"type": "item_update", "uuid": "318d16a7-0c88-46e0-9eb5-cde6c72477c8", "args": {"id": 33548400, "priority": 2}}]'
{ ...
  "SyncStatus": {"318d16a7-0c88-46e0-9eb5-cde6c72477c8": "ok"},
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
assigned_by_uid | The id of user who assigns current task. Makes sense for shared projects only. Accepts `0` or any user id from the list of project collaborators. If this value is unset or invalid, it will automatically be set up by your uid.
responsible_uid | The id of user who is responsible for accomplishing the current task. Makes sense for shared projects only. Accepts `0` or any user id from the list of project collaborators. If this value is unset or invalid, it will automatically be set up by null.

## Delete items

> An example of deleting a task:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "item_delete", "uuid": "f8539c77-7fd7-4846-afad-3b201f0be8a5", "args": {"ids": [33548400]}}]'
{ ...
  "SyncStatus": {"f8539c77-7fd7-4846-afad-3b201f0be8a5": {"33548400": "ok"}},
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
    -d commands='[{"type": "item_move", "uuid": "818f108a-36d3-423d-857f-62837c245f3b", "args": {"project_items": {"128501470": [33548400]}, "to_project": 128501607}}]'
{ ...
  "SyncStatus": {"818f108a-36d3-423d-857f-62837c245f3b": {"33548400": "ok"}},
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
    -d commands='[{"type": "item_complete", "uuid": "a74bfb5c-5f1d-4d14-baea-b7415446a871", "args": {"project_id": 128501470, "ids": [33548400]}}]'
{ ...
  "SyncStatus": {"a74bfb5c-5f1d-4d14-baea-b7415446a871": {"33548400": "ok"}},
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
    -d commands='[{"type": "item_uncomplete", "uuid": "710a60e1-174a-4313-bb9f-4df01e0349fd", "args": {"project_id": 128501470, "ids": [33548400]}}]'
{ ...
  "SyncStatus": {"710a60e1-174a-4313-bb9f-4df01e0349fd": {"33548400": "ok"}},
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
restore_state | A dictionary, where item id is the key, and its value is a list of four elements, whether the item is in history, whether it is checked, its order and indent: `item_id: [in_history, checked, item_order, indent]`


## Complete a recurring task

> An example of completing a recurring task:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "item_update_date_complete", "uuid": "c5888360-96b1-46be-aaac-b49b1135feab", "args": {"id": 33548400, "new_data_utc": "2014-10-30T23:59", "date_string": "every day", "is_forward": 1}}]'
{ ...
  "SyncStatus": {"c5888360-96b1-46be-aaac-b49b1135feab": "ok"},
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
    -d commands='[{"type": "item_update_orders_indents", "uuid": "a2bf0c06-f834-4442-99ab-b86fdfc66ed5", "args": {"ids_to_orders_indents": {"33548400": [1, 1]}}}]'
{ ...
  "SyncStatus": {"a2bf0c06-f834-4442-99ab-b86fdfc66ed5": {"33548400": "ok"}},
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
    -d commands='[{"type": "item_update_day_orders", "uuid": "dbeb40fc-905f-4d8a-8bae-547d3bbd6e91", "args": {"ids_to_orders": {"33548400": 1}}}]'
{ ...
  "SyncStatus": {"dbeb40fc-905f-4d8a-8bae-547d3bbd6e91": {"33548400": "ok"}},
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
