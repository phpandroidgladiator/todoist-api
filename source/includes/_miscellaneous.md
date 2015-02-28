# Miscellaneous

## Get redirect link

> An example of getting the user's absolute URL to redirect or to open in a browser:

```shell
$ curl https://todoist.com/API/v6/get_redirect_link \
    -d token=0123456789abcdef0123456789abcdef01234567
{"link": "https:\/\/local.todoist.com\/secureRedirect?path=%2Fapp&token=abcdefghijklmnopqrstuvwxyz01234567890ABCDEFGHIJKLMNOPQRSTUVWXYZ"}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.get_redirect_link()
{'link': 'https:\/\/local.todoist.com\/secureRedirect?path=%2Fapp&token=abcdefghijklmnopqrstuvwxyz.0123456789.ABCDEFGHIJKLMNOPQRSTUVWXYZ'}
```

Get the user's absolute URL to redirect or to open in a browser. For the first time the link logs in the user automatically and performs a redirect to a given page, but once used, the link keeps working as a plain redirect. The link keeps working as a login link for as long as one week after it's been issued.

### Required parameters

Parameter | Description
--------- | -----------
token | The user's token (received on login).

### Optional parameters

Parameter | Description
--------- | -----------
path | The path to redirect user's browser (default is "/app").
hash | The hash part of the path to redirect user's browser.

## Get productivity stats

> An example of getting the user's productivity stats:

```shell
$ curl https://todoist.com/API/v6/get_productivity_stats \
    -d token=0123456789abcdef0123456789abcdef01234567
{
  "karma_last_update": 50.0,
  "karma_trend": "up",
  "days_items": [
    { "date": "2014-11-03",
      "items": [],
      "total_completed": 0 },
  ],
  "completed_count": 0,
  "karma_update_reasons": [
    { "positive_karma_reasons": [4],
      "new_karma": 50.0,
      "negative_karma": 0.0,
      "positive_karma": 50.0,
      "negative_karma_reasons": [],
      "time": "Mon 20 Oct 2014 12:06:52"}
  ],
  "karma": 50.0,
  "week_items": [
    { "date": "2014-11-03\/2014-11-09",
      "items": [],
      "total_completed": 0 },
  ],
  "project_colors": {},
  "karma_graph": "https:\/\/todoist.com\/chart?cht=lc&chs=255x70&chd=s:A9&chco=dd4b39&chf=bg,s,ffffff&chxt=x,y&chxl=0:%7cMo%2C%2020%7c%7c1:%7c0%7c50&chxs=0,999999%7c1,999999",
  "goals": {
    "karma_disabled": 0,
    "user_id": 4,
    "max_weekly_streak": { 
      "count": 0,
      "start": "",
      "end": ""
    },
    "ignore_days": [6, 7],
    "vacation_mode": 0,
    "current_weekly_streak": {
      "count": 0,
      "start": "",
      "end": ""
    },
    "current_daily_streak": {
      "count": 0,
      "start": "",
      "end": ""
    },
    "weekly_goal": 25,
    "max_daily_streak": {
      "count": 0,
      "start": "",
      "end": ""
    },
    "daily_goal": 5
  }
}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.get_productivity_stats()
{
  'karma_last_update': 50.0,
  'karma_trend': 'up',
  'days_items': [
    { 'date': '2014-11-03',
      'items': [],
      'total_completed': 0}
  ],
  'completed_count': 0,
  'karma_update_reasons': [
    { 'positive_karma_reasons': [4],
      'new_karma': 50.0,
      'negative_karma': 0.0,
      'positive_karma': 50.0,
      'negative_karma_reasons': [],
      'time': 'Mon 20 Oct 2014 12:06:52' }
  ],
  'karma': 50.0,
  'week_items': [
    { 'date': '2014-11-03/2014-11-09',
      'items': [],
      'total_completed': 0 },
  ],
  'project_colors': {},
  'karma_graph': 'https://todoist.com/chart?cht=lc&chs=255x70&chd=s:A9&chco=dd4b39&chf=bg,s,ffffff&chxt=x,y&chxl=0:%7cMo%2C%2020%7c%7c1:%7c0%7c50&chxs=0,999999%7c1,999999',
  'goals': {
    'karma_disabled': 0,
    'user_id': 4,
    'max_weekly_streak': {
      'count': 0,
      'start': '',
      'end': ''
    },
    'ignore_days': [6, 7],
    'vacation_mode': 0,
    'current_weekly_streak': {
      'count': 0,
      'start': '',
      'end': ''
    },
    'current_daily_streak': {
      'count': 0,
      'start': '',
      'end': ''
    },
    'weekly_goal': 25,
    'max_daily_streak': {
      'count': 0,
      'start': '',
      'end': ''
    },
    'daily_goal': 5
  }
}

```

Get the user's productivity stats.

### Required parameters

Parameter | Description
--------- | -----------
token | The user's token (received on login).

## Update notification settings

> An example of updating the user's notification settings

```shell
$ curl https://todoist.com/API/v6/update_notification_setting \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d notification_type=item_completed \
    -d service=email \
    -d dont_notify=1
{
  "user_left_project": {
    "notify_push": true,
    "notify_email": true
  },
  "biz_trial_will_end": {
    "notify_push": true,
    "notify_email": true
  },
  "biz_trial_enter_cc": {
    "notify_push": true,
    "notify_email": true
  },
  "item_completed": {
    "notify_push": true,
    "notify_email": false
  },
  "share_invitation_rejected": {
    "notify_push": true,
    "notify_email": true
  },
  "note_added": {
    "notify_push": true,
    "notify_email": true
  },
  "biz_account_disabled": {
    "notify_push": true,
    "notify_email": true
  },
  "biz_invitation_rejected": {
    "notify_push": true,
    "notify_email": true
  },
  "item_uncompleted": {
    "notify_push": true,
    "notify_email": true
  },
  "item_assigned": {
    "notify_push": true,
    "notify_email": true
  },
  "share_invitation_accepted": {
    "notify_push": true,
    "notify_email": true
  },
  "user_removed_from_project": {
    "notify_push": true,
    "notify_email": true
  },
  "biz_invitation_accepted": {
    "notify_push": true,
    "notify_email": true
  },
  "biz_payment_failed": {
    "notify_push": true,
    "notify_email": true
  }
}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.update_notification_setting('item_completed', 'email', 1)
{
  'biz_invitation_rejected': {
    'notify_push': True,
    'notify_email': True
  },
  'user_left_project': {
    'notify_push': True,
    'notify_email': True
  },
  'note_added': {
    'notify_push': True,
    'notify_email': True
  },
  'biz_trial_enter_cc': {
    'notify_push': True,
    'notify_email': True
  },
  'item_completed': {
    'notify_push': True,
    'notify_email': False
  },
  'biz_trial_will_end': {
    'notify_push': True,
    'notify_email': True
  },
  'biz_account_disabled': {
    'notify_push': True,
    'notify_email': True
  },
  'share_invitation_rejected': {
    'notify_push': True,
    'notify_email': True
  },
  'item_uncompleted': {
    'notify_push': True,
    'notify_email': True
  },
  'item_assigned': {
    'notify_push': True,
    'notify_email': True
  },
  'share_invitation_accepted': {
    'notify_push': True,
    'notify_email': True
  },
  'user_removed_from_project': {
    'notify_push': True,
    'notify_email': True
  },
  'biz_invitation_accepted': {
    'notify_push': True,
    'notify_email': True
  },
  'biz_payment_failed': {
    'notify_push': True,
    'notify_email': True
  }
}
```

Update the user's notification settings.

### Required parameters

Parameter | Description
--------- | -----------
token | The user's token (received on login).
notification_type | The notification type.
service | The service type, which can be `email` or `push`.
dont_notify | Should we notify on this service? Can be `1` or `0`.

## Get all completed items

> An example of getting the user's completed tasks

```shell
$ curl https://todoist.com/API/v6/get_all_completed_items \
    -d token=0123456789abcdef0123456789abcdef01234567
{
  "items": [
    { "content": "Item11",
      "meta_data": null,
      "user_id": 1855589,
      "task_id": 33511505,
      "note_count": 0,
      "project_id": 128501470,
      "completed_date": "Tue 17 Feb 2015 15:40:41 +0000",
      "id": 33511505
    }
  ],
  "projects": {
    "128501470":
    { "last_updated": "",
      "color": 7,
      "collapsed": 0,
      "archived_date": null,
      "indent": 1,
      "cache_count": 0,
      "is_deleted": 0,
      "id": 128501470,
      "user_id": 1855589,
      "name": "Project1",
      "item_order": 36,
      "archived_timestamp": 0,
      "is_archived": 0 }
  }
}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.get_all_completed_items()
{
  'items': [
    { 'user_id': 1855589,
      'task_id': 33511505,
      'note_count': 0,
      'completed_date': 'Tue 17 Feb 2015 15:40:41 +0000',
      'content': 'Item1',
      'meta_data': None,
      'project_id': 128501470,
      'id': 33511505},
  ],
  'projects': {
    '128501470':
      { 'last_updated': '',
        'name': 'Inbox',
        'user_id': 1855589,
        'color': 7,
        'is_deleted': 0,
        'collapsed': 0,
        'inbox_project': True,
        'archived_date': None,
        'item_order': 36,
        'is_archived': 0,
        'cache_count': 0,
        'indent': 1,
        'archived_timestamp': 0,
        'id': 128501470 }
  }
}
```

Get all the user's completed items (tasks).

### Required parameters

Parameter | Description
--------- | -----------
token | The user's token (received on login).

### Optional parameters

Parameter | Description
--------- | -----------
project_id | Filter the tasks by `project_id`.
limit | The number of items to return (default is `30`, while maximum is `50`).
offset | Can be used for pagination, when more than the `limit` tasks are returned.
from_date | Return items with a completed date same or older than `from_date` (formated as `2007-4-29T10:13`).
to_date | Return items with a completed date newer than `to_date` (formated as `2007-4-29T10:13`).

## Add item

> An example of adding a task:

```shell
$ curl https://todoist.com/API/v6/add_item \
    -d token=0123456789abcdef0123456789abcdef01234567
    -d content=Task1
{ "due_date": null,
  "assigned_by_uid": 1855589,
  "is_archived": 0,
  "labels": [],
  "sync_id": null,
  "in_history": 0,
  "has_notifications": 0,
  "date_added": "Wed 18 Feb 2015 11:09:11 +0000",
  "indent": 1,
  "children": null,
  "content": "Task1",
  "is_deleted": 0,
  "user_id": 1855589,
  "due_date_utc": null,
  "id": 33548400,
  "priority": 4,
  "item_order": 1,
  "responsible_uid": null,
  "project_id": 128501411,
  "collapsed": 0,
  "checked": 0,
  "date_string": "" }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.add_item("Task1")
{ 'is_archived': 0,
  'labels': [],
  'sync_id': None,
  'in_history': 0,
  'checked': 0,
  'id': 33548400,
  'priority': 4,
  'user_id': 1855589,
  'date_added': 'Wed 18 Feb 2015 11:09:11 +0000',
  'children': None,
  'content': 'Task1',
  'item_order': 1,
  'project_id': 128501411,
  'date_string': '',
  'due_date': None,
  'assigned_by_uid': 1855589,
  'collapsed': 0,
  'has_notifications': 0,
  'indent': 1,
  'is_deleted': 0,
  'due_date_utc': None,
  'responsible_uid': None }
```

Add a new task to a project.  Note, that this is provided as a helper method, a shortcut, to quickly add a task without going through the `Sync workflow` described in a previous section.

### Required parameters

Parameter | Description
--------- | -----------
token | The user's token (received on login).

### Optional parameters

Parameter | Description
--------- | -----------
project_id | The id of the project to add the task to (defaults to `Inbox` project).
date_string | The date of the task, added in free form text, for example it can be `every day @ 10`. Look at our reference to see [which formats are supported](https://todoist.com/Help/timeInsert).
priority | The priority of the task (a number between `1` and `4`, `4` for very urgent and `1` for natural).
indent | The indent of the item (a number between `1` and `4`, where `1` is top-level).
item_order | The order of the task.
children | The tasks child tasks (a list of task ids such as `[13134,232345]`)
labels | The tasks labels (a list of label ids such as `[2324,2525]`)
assigned_by_uid | The id of user who assigns current task. Makes sense for shared projects only. Accepts `0` or any user id from the list of project collaborators. If this value is unset or invalid, it will automatically be set up by your uid.
responsible_uid | The id of user who is responsible for accomplishing the current task. Makes sense for shared projects only. Accepts `0` or any user id from the list of project collaborators. If this value is unset or invalid, it will automatically be set up by null.
note | Add a note directly to the task, note is a string of the content.
