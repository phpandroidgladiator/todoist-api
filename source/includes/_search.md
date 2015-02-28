# Data Query and Search

> On success, an HTTP 200 OK with a JSON object with the tasks found is returned:

```shell
     { "due_date": "Wed 08 Oct 2014 20:59:59 +0000",
        "is_deleted": 0,
        "assigned_by_uid": 1855589,
        "is_archived": 0,
        "labels": [],
        "sync_id": null,
        "day_order": 0,
        "postpone_count": 0,
        "in_history": 0,
        "has_notifications": 0,
        "indent": 1,
        "date_added": "Tue 07 Oct 2014 06:11:06 +0000",
        "user_id": 1855589,
        "children": null,
        "priority": 1,
        "complete_count": 0,
        "checked": 0,
        "id": 35825425,
        "content": "Task2",
        "item_order": 2,
        "seq_no": 2306453653,
        "responsible_uid": null,
        "project_id": 128501470,
        "collapsed": 0,
        "date_string": "8 Oct" },
      { "due_date": "Wed 08 Oct 2014 20:59:59 +0000",
        "is_deleted": 0,
        "assigned_by_uid": 1855589,
        "is_archived": 0,
        "labels": [],
        "sync_id": null,
        "day_order": 1,
        "postpone_count": 0,
        "in_history": 0,
        "has_notifications": 0,
        "indent": 1,
        "date_added": "Tue 07 Oct 2014 06:20:48 +0000",
        "user_id": 1855589,
        "children": null,
        "priority": 1,
        "complete_count": 0,
        "checked": 0,
        "id": 35826737,
        "content": "Task 3",
        "item_order": 3,
        "seq_no": 2306541514,
        "responsible_uid": null,
        "project_id": 128501470,
        "collapsed": 0,
        "date_string": "8 Oct"}
    ]
  },
  {
    "query": "p1",
    "type": "priority",
    "data": [
      { "due_date": null,
        "is_deleted": 0,
        "assigned_by_uid": 1855589,
        "is_archived": 0,
        "labels": [],
        "sync_id": null,
        "postpone_count": 5,
        "in_history": 0,
        "has_notifications": 0,
        "indent": 1,
        "date_added": "Fri 26 Sep 2014 11:54:48 +0000",
        "user_id": 1855589,
        "children": null,
        "priority": 4,
        "complete_count": 1,
        "checked": 0,
        "id": 33548400,
        "content": "Task1",
        "item_order": 1,
        "seq_no": 2306454606,
        "responsible_uid": null,
        "project_id": 128501470,
        "collapsed": 0,
        "date_string": "" }
    ]
  }
]
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.query(['tomorrow', 'p1'])
[
  {
    'data': [
      { 'assigned_by_uid': 1855589,
        'checked': 0,
        'children': None,
        'collapsed': 0,
        'complete_count': 0,
        'content': 'Task2',
        'date_added': 'Tue 07 Oct 2014 06:11:06 +0000',
        'date_string': '8 Oct',
        'day_order': 0,
        'due_date': 'Wed 08 Oct 2014 20:59:59 +0000',
        'has_notifications': 0,
        'id': 35825425,
        'in_history': 0,
        'indent': 1,
        'is_archived': 0,
        'is_deleted': 0,
        'item_order': 2,
        'labels': [],
        'postpone_count': 0,
        'priority': 1,
        'project_id': 128501470,
        'responsible_uid': None,
        'seq_no': 2306453653L,
        'sync_id': None,
        'user_id': 1855589 },
      { 'assigned_by_uid': 1855589,
        'checked': 0,
        'children': None,
        'collapsed': 0,
        'complete_count': 0,
        'content': 'Task 3',
        'date_added': 'Tue 07 Oct 2014 06:20:48 +0000',
        'date_string': '8 Oct',
        'day_order': 1,
        'due_date': 'Wed 08 Oct 2014 20:59:59 +0000',
        'has_notifications': 0,
        'id': 35826737,
        'in_history': 0,
        'indent': 1,
        'is_archived': 0,
        'is_deleted': 0,
        'item_order': 3,
        'labels': [],
        'postpone_count': 0,
        'priority': 1,
        'project_id': 128501470,
        'responsible_uid': None,
        'seq_no': 2306541514L,
        'sync_id': None,
        'user_id': 1855589 }
    ],
    'query': 'tomorrow',
    'type': 'date'
  },
  {
    'data': [
      { 'assigned_by_uid': 1855589,
        'checked': 0,
        'children': None,
        'collapsed': 0,
        'complete_count': 1,
        'content': 'Task1',
        'date_added': 'Fri 26 Sep 2014 11:54:48 +0000',
        'date_string': '',
        'due_date': None,
        'has_notifications': 0,
        'id': 33548400,
        'in_history': 0,
        'indent': 1,
        'is_archived': 0,
        'is_deleted': 0,
        'item_order': 1,
        'labels': [],
        'postpone_count': 5,
        'priority': 4,
        'project_id': 128501470,
        'responsible_uid': None,
        'seq_no': 2306454606L,
        'sync_id': None,
        'user_id': 1855589}],
    'query': 'p1',
    'type': 'priority'
  }
]
```

You can query after date, priority or labels.

### Required arguments

Argument | Description
-------- | -----------
token | The user's token (received on login).
queries | A JSON list of queries to search. [Examples of searches](https://todoist.com/Help/timeQuery) can be found in the Todoist help page.

### Optional arguments

Argument | Description
-------- | -----------
as_count | If set to `1` then no data will be returned, instead the count of tasks matching will be returned.


