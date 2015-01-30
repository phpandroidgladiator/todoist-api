# Data

## Same-origin policy

> Script for solving cross-domain security policy:

```js
var script = document.createElement('script');
script.type = 'text/javascript';
script.src = 'https://api.todoist.com/API/...&callback=callbackFunction';
document.getElementsByTagName('head')[0].appendChild(script);
```

> The response data will look like this (callbackFunction will be called):

```js
callbackFunction({ JSON data here });
```

You can't use AJAX to directly communicate with Todoist, this is due to cross domain security policy of browsers. You can solve this by communicating with Todoist using a script tag.

## Workflow

The API has been simplified (in version 6), and almost all interactions with
the Todoist server can be done with a single call, which can be used to get the
full model (projects, items, etc.), and then update it, or perform changes to
it.

Then there are a few more auxiliary calls that can perform some actions that
are not related to the full model, but are nonetheless useful.

### Available calls

Call | Description
---- | -----------
sync | Retrieves or sends data.
query | Query after date, priority or labels.
add_item | Adds a new item.
upload_file | Uploads a file.
login_oauth | Logins to the server OAuth2.
login_plain | Logins to the server using plain email/password.
register | Registers a new account.
delete_user | Deletes an existing account.
get_redirect_link | Gets the redirect link.
get_productivity_stats | Gets the productivity stats.
update_notification_settings | Updates the notification settings.

## Errors

Another improvement in the version 6 of the API is the better and more
consistent error reporting.  So now the server uses the HTTP status codes to
indicate result or failure of a request.  And as is usually customary in web servers,
a 2xx code indicates success, a 4xx code an error due to incorrect user
provided information, and a 5xx code an internal, possibly temporary, error.

### Status codes

Status code | Description
------------|------------
200 OK | The request was processed successfuly.
400 Bad Request | The request was incorrect.
500 Internal Server Error | The request failed due to a server error.

### Return values

> An example of an error return value:

```json
{ "error": [18, "Required arguments are missing"]}
```

In any case, a JSON object is always returned.  So if the request was succesful, a JSON object with the relevant to the request data is returned or if there is no data to return a simple `"ok"`, while if the request failed, a JSON object of the format `{"error":[error_code, error_string]}` is returned (where `error_code` is a number, and `error_string` a description).

> An example of a single request sync return value:

```json
{
  "SyncStatus": [{ "timestamp": "1411653990.1", "status": "ok"}]
}
```

> An example of a multiple requests sync return value:

```json
{
  "SyncStatus": [
    { "timestamp": "1411653991.1", "status": "ok"},
    { "timestamp": "1411653991.2",
      "status": {"error": [15, "Invalid temporary id"]} },
  ]
}
```

> An example of a single request operating on multiple objects return value:

```json
{
  "SyncStatus": [
    { "timestamp": "1411653992.2",
      "status": [
        {"128501470": "ok"},
        {"128501607": {"error": [20, "Project not found"]}}
      ]
    }
  ]
}
```

Also, specifically for the case of the `sync` request, where multiple requests can be sent with a single call by the client, there is a special object reserved for indicating the return value of each specific request, the `SyncStatus`.

The `SyncStatus` is a list of the return values of each of the multiple
requests.  Each return value in this list contains a `timestamp` field which
matches the timestamp of each request, and a `status` field which is the actual
return value of each request.

If one or more of the multiple requests operate in multiple objects, then the `status` field is itself a list of return values one for each of the multiple objects, where each object is distinguished by its id.

## Retrieve data

> An example of the JSON object returned, of a new user's empty account complete data:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d seq_no=0
{
  "seq_no_global": 2180537512,
  "Collaborators": [],
  "DayOrdersTimestamp": "1344642991.1",
  "Notes": [],
  "Labels": [],
  "UserId": 1855589,
  "CollaboratorStates": [],
  "DayOrders": {},
  "LiveNotifications": [],
  "seq_no": 2180537512,
  "User": {
    "start_page": "overdue, 7 days",
    "join_date": "Wed 30 Apr 2014 13:24:38 +0000",
    "is_premium": false,
    "sort_order": 0,
    "full_name": "Example User",
    "has_push_reminders": false,
    "timezone": "Europe\/Athens",
    "id": 1855589,
    "next_week": 1,
    "completed_count": 20,
    "tz_offset": ["+03:00", 3, 0, 1],
    "email": "me@example.com",
    "start_day": 1,
    "is_dummy": 0,
    "inbox_project": 128501411,
    "time_format": 0,
    "image_id": null,
    "beta": 0,
    "premium_until": null,
    "business_account_id": null,
    "mobile_number": null,
    "mobile_host": null,
    "date_format": 0,
    "karma_trend": "-",
    "token": "0123456789abcdef0123456789abcdef01234567",
    "karma": 684.0,
    "is_biz_admin": false,
    "default_reminder": null
  },
  "Filters": [
    { "user_id": 1855589,
      "name": "No due date",
      "color": 6,
      "is_deleted": 0,
      "item_order": 8,
      "query": "no date",
      "id": 1855589 },
    { "user_id": 1855589,
      "name": "View all",
      "color": 6,
      "is_deleted": 0,
      "item_order": 7,
      "query": "view all",
      "id": 1855589 },
    { "user_id": 1855589,
      "name": "Priority 4",
      "color": 6,
      "is_deleted": 0,
      "item_order": 6,
      "query": "priority 4",
      "id": 1855589 },
    { "user_id": 1855589,
      "name": "Priority 3",
      "color": 6,
      "is_deleted": 0,
      "item_order": 5,
      "query": "priority 3",
      "id": 1855589 },
    { "user_id": 1855589,
      "name": "Priority 2",
      "color": 6,
      "is_deleted": 0,
      "item_order": 4,
      "query": "priority 2",
      "id": 1855589 },
    { "user_id": 1855589,
      "name": "Priority 1",
      "color": 6,
      "is_deleted": 0,
      "item_order": 3,
      "query": "priority 1",
      "id": 1855589 },
    { "user_id": 1855589,
      "name": "Assigned to others",
      "color": 12,
      "is_deleted": 0,
      "item_order": 2,
      "query": ":to_others:",
      "id": 1855589 },
    { "user_id": 1855589,
      "name": "Assigned to me",
      "color": 12,
      "is_deleted": 0,
      "item_order": 1,
      "query": ":to_me:",
      "id": 1855589 }
  ],
  "Items": [],
  "WebStaticVersion": 305,
  "Reminders": [],
  "Projects": [
    { "last_updated": "1344642991.1",
      "color": 7,
      "collapsed": 0,
      "inbox_project": true,
      "archived_date": null,
      "archived_timestamp": 0,
      "cache_count": 0,
      "id": 1855589,
      "indent": 1,
      "name": "Inbox",
      "user_id": 1855589,
      "is_deleted": 0,
      "item_order": 0,
      "shared": false,
      "is_archived": 0}
  ],
  "LiveNotificationsLastRead": 0
}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.get()
{ 'CollaboratorStates': [],
  'Collaborators': [],
  'DayOrders': {},
  'DayOrdersTimestamp': '1344642991.1',
  'Filters': [
    { 'color': 6,
      'id': 4638883,
      'is_deleted': 0,
      'item_order': 8,
      'name': 'No due date',
      'query': 'no date',
      'user_id': 1855589},
    { 'color': 6,
      'id': 4638882,
      'is_deleted': 0,
      'item_order': 7,
      'name': 'View all',
      'query': 'view all',
      'user_id': 1855589},
    { 'color': 6,
      'id': 4638881,
      'is_deleted': 0,
      'item_order': 6,
      'name': 'Priority 4',
      'query': 'priority 4',
      'user_id': 1855589},
    { 'color': 6,
      'id': 4638880,
      'is_deleted': 0,
      'item_order': 5,
      'name': 'Priority 3',
      'query': 'priority 3',
      'user_id': 1855589},
    { 'color': 6,
      'id': 4638879,
      'is_deleted': 0,
      'item_order': 4,
      'name': 'Priority 2',
      'query': 'priority 2',
      'user_id': 1855589},
    { 'color': 6,
      'id': 4638878,
      'is_deleted': 0,
      'item_order': 3,
      'name': 'Priority 1',
      'query': 'priority 1',
      'user_id': 1855589},
    { 'color': 12,
      'id': 4638876,
      'is_deleted': 0,
      'item_order': 2,
      'name': 'Assigned to others',
      'query': ':to_others:',
      'user_id': 1855589},
    { 'color': 12,
      'id': 4638874,
      'is_deleted': 0,
      'item_order': 1,
      'name': 'Assigned to me',
      'query': ':to_me:',
      'user_id': 1855589},
  ],
  'Items': [],
  'Labels': [],
  'LiveNotifications': [],
  'LiveNotificationsLastRead': 0,
  'Notes': [],
  'Projects': [
    { 'archived_date': None,
       'archived_timestamp': 0,
       'cache_count': 0,
       'collapsed': 0,
       'color': 7,
       'id': 128501411,
       'inbox_project': True,
       'indent': 1,
       'is_archived': 0,
       'is_deleted': 0,
       'item_order': 0,
       'last_updated': '1344642991.1',
       'name': 'Inbox',
       'shared': False,
       'user_id': 1855589}
  ],
  'Reminders': [],
  'User': {
    'beta': 0,
    'business_account_id': None,
    'completed_count': 20,
    'date_format': 0,
    'default_reminder': None,
    'email': 'me@example.com',
    'full_name': 'Example User',
    'has_push_reminders': False,
    'id': 1855589,
    'image_id': None,
    'inbox_project': 128501411,
    'is_biz_admin': False,
    'is_dummy': 0,
    'is_premium': False,
    'join_date': 'Wed 30 Apr 2014 13:24:38 +0000',
    'karma': 684.0,
    'karma_trend': '-',
    'mobile_host': None,
    'mobile_number': None,
    'next_week': 1,
    'premium_until': None,
    'sort_order': 0,
    'start_day': 1,
    'start_page': 'overdue, 7 days',
    'time_format': 0,
    'timezone': 'Europe/Athens',
    'token': '0123456789abcdef0123456789abcdef01234567',
    'tz_offset': ['+03:00', 3, 0, 1]
  },
  'UserId': 1855589,
  'WebStaticVersion': 305,
  'seq_no': 2180537512L,
  'seq_no_global': 2180537512L
}
```

The `sync` API call is used to retrieve data (both all and only updated).

### Required parameters

Parameter | Description
--------- | -----------
api_token | User's API token (returned on successful login). Else the session cookie is used.
seq_no | Sequence number. On the initial request you should pass `0`. On all other requests you should pass the last `seq_no` you received from the server.

### Optional parameters

Parameter | Description
--------- | -----------
day_orders_timestamp | The `sync` API requests return `DayOrdersTimestamp` that specifies when the day orders were last updated. If you omit `day_orders_timestamp` then none of then will be fetched. If you specify `day_orders_timestamp` then day orders will be returned if your timestamp is different from the servers. If you send `day_orders_timestamp` and the day orders have not been updated then the server won't return the `DayOrders` entry at all.
include_notification_settings | Include notification settings (`SettingsNotifications`). This is needed on platforms that implement native notifications.
resource_types | An optional parameter which is useful if you don't need a complete result, but want to have just a subset. It can be useful for speeding up the load of most important user's data (like the list of projects and tasks) and to get the rest of the data asynchronously later on. It should be a JSON-encoded list of strings. For example, `["projects", "labels", "filters"]`. Below is the list of recognizable values for strings: `projects` for the list of projects and tasks, `labels` for the list of labels, `day_orders` for the list of day orders, `filters` for the list of filters, `reminders` for the list of reminders,  `collaborators` for the list of collaborators, `notes` for the list of notes, `live_notifications` for the list of live notifications, `share_invitations` for the list of invitations.

### Explanation of data returned

A brief explanation of what each object returned contains, while each of them will be later described in more detail.

Data | Description
---- | -----------
Users | A JSON object with a user's information. Only included if `seq_no` is different.
Projects |  A list of JSON objects of projects.
Items | A JSON list of objects of items.
Labels | A JSON list of objects (including things like color, id etc.)
Filters | A JSON list of custom filters.
DayOrders |  A JSON object specifying the order of items in daily agenda. If `DayOrdersTimestamp` is sent and day orders have not been updated then `DayOrders` won't be returned at all!
DayOrdersTimestamp | A string specifying when day orders where last updated. Use this to not fetch day orders on every request.
Reminders |  A JSON list containing all reminders for all items in the project.
Collaborators | A JSON object containing all collaborators for all shared projects. The `projects` field contains the list of all shared projects, where the user acts as one of collaborators.
CollaboratorsStates | A JSON list specifying the state of each collaborator in each project. The state can be invited, active, inactive, deleted.
LiveNotifications | A JSON list of notifications for the user. On the initial request (`seq_no=0`) all notifications are returned. Afterwards only the updated.
LiveNotificationsLastRead | What is the last `seq_no` the user has seen? This is used to implement unread notifications.
Settings | Includes user's settings as a JSON object, including things like start page query and timezone information. Only included if `seq_no` is different.
SettingsNotifications | The same data as the `getNotificationSettings` API call returns. This is needed on platforms that implement native notifications. Only included if `seq_no` is different.

## Send data

> Example of creating a new project, where an HTTP 200 OK with a JSON object of temporary id to real id mappings is returned (this will be explained later):

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "project_add", "temp_id": "$1411653993.1", "timestamp": "1411653993.1", "args": {"name": "Project1", "item_order": 1, "indent": 1, "color": 1}}]'
{ "seq_no_global": 2180537513,
  "DayOrdersTimestamp": "1344642991.1",
  "UserId": 1855589,
  "seq_no": 2180537513,
  "SyncStatus": [
    {"status": "ok", "timestamp": "1411653993.1"}
  ],
  "TempIdMapping": {"$1411653993.1": 128501470},
  "User": {
    "start_page": "overdue, 7 days",
    "join_date": "Wed 30 Apr 2014 13:24:38 +0000",
    "is_premium": false,
    "sort_order": 0,
    "full_name": "Example User",
    "has_push_reminders": false,
    "timezone": "Europe\/Athens",
    "id": 1855589,
    "next_week": 1,
    "completed_count": 20,
    "tz_offset": ["+03:00", 3, 0, 1],
    "email": "me@example.com",
    "start_day": 1,
    "is_dummy": 0,
    "inbox_project": 128501411,
    "time_format": 0,
    "image_id": null,
    "beta": 0,
    "premium_until": null,
    "business_account_id": null,
    "mobile_number": null,
    "mobile_host": null,
    "date_format": 0,
    "karma_trend": "-",
    "token": "0123456789abcdef0123456789abcdef01234567",
    "karma": 684.0,
    "is_biz_admin": false,
    "default_reminder": null
  },
}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.sync(items_to_sync=[{'type': 'project_add', 'temp_id': '$1411653993.1', 'timestamp': '1411653993.1', 'args': {'name': 'Project1', 'item_order': 1, 'indent': 1, 'color': 1}}]
{ 'DayOrdersTimestamp': '1344642991.1',
  'SyncStatus': [
    {'status': 'ok', 'timestamp': '1411653993.1'}
  ],
  'TempIdMapping': {'$1411653993.1': 128501470},
  'User': {
    'beta': 0,
    'business_account_id': None,
    'completed_count': 20,
    'date_format': 0,
    'default_reminder': None,
    'email': 'me@example.com',
    'full_name': 'Example User',
    'has_push_reminders': False,
    'id': 1855589,
    'image_id': None,
    'inbox_project': 128501411,
    'is_biz_admin': False,
    'is_dummy': 0,
    'is_premium': False,
    'join_date': 'Wed 30 Apr 2014 13:24:38 +0000',
    'karma': 684.0,
    'karma_trend': '-',
    'mobile_host': None,
    'mobile_number': None,
    'next_week': 1,
    'premium_until': None,
    'sort_order': 0,
    'start_day': 1,
    'start_page': 'overdue, 7 days',
    'time_format': 0,
    'timezone': 'Europe/Athens',
    'token': '0123456789abcdef0123456789abcdef01234567',
    'tz_offset': ['+03:00', 3, 0, 1]
  },
  'UserId': 1855589,
  'seq_no': 2180537513L,
  'seq_no_global': 2180537513L,
}
```

The `sync` API call can also take a list of JSON object commands which specify which changes should be done to the model. These changes could be adding projects, deleting tasks, adding notes etc.

It supports two big features which can make syncing easier: temporary ids, that is the ability to use and resolve temporary ids, and duplication protection which ensures that a command isn't run two times.

### Required parameters

Parameter | Description
--------- | -----------
items_to_sync | A list of JSON object commands. These commands are specified later in the documentation.
api_token | User's API token (returned on successful login). Else the session cookie is used.

### Explanation of extra data returned

When there are items to be synced to the server, some extra data are returned.

Data | Description
---- | -----------
TempIdMapping | A JSON list containing all the temporary ids mapped to real ids.
SyncStatus | A JSON list containing the return value of each of the items that where synced.

## Timestamps

Each `timestamp` should be based on the unix timestamp of when the command was created. This is used for duplication protection, i.e. we want to ensure that same command isn't executed twice! This check works by keeping track of command types, temporary ids and timestamps. By just supplying timestamps the system does these checks automatically and will ignore duplicated commands.

## Temporary ids

> An example that shows how temporary ids can be used and referenced:

```json
[
  { "type": "project_add",
    "temp_id": "$1411654161.1",
    "args": {
      "name": "Project3",
      "item_order": 1,
      "indent": 1,
      "color": 1
    },
    "timestamp": "1411654161.1"
  },
  {
    "type": "project_update",
    "args": {
      "id": "$1411654161.1",
      "color": 2
    },
    "timestamp": "1411654193.1"
  }
]
```

>  You can see that the project_add command has a temp_id property, which specifies which temporary id this new project has. The project_update command later references this temporary id. The API will automatically resolve these ids. Additionally `sync` will return the real id in the result. Remember to update your local model with these real ids e.g.:

```json
{"$1411654161.1": 128501682}
```

Your application will use temporary ids and the Sync API has special support for them. We suggest that you use unix timestamps to generate these ids to give them uniqueness (maybe also prefixing them with a special number).

While the system remembers temporary ids and their mappings to real ids, it's important to use real ids when they are available to you (typically after a sync). This is important since the API only remembers the last 500 temporary ids for each user!

## Date query and search

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
        "mm_offset": 180,
        "is_dst": null,
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
        "mm_offset": 180,
        "is_dst": null,
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
        "mm_offset": 180,
        "is_dst": null,
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
        'is_dst': None,
        'item_order': 2,
        'labels': [],
        'mm_offset': 180,
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
        'is_dst': None,
        'item_order': 3,
        'labels': [],
        'mm_offset': 180,
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
        'is_dst': None,
        'item_order': 1,
        'labels': [],
        'mm_offset': 180,
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
js_date | If `js_date` is set to `1` dates will be formated as `new Date("Sun Apr 29 2007 23:59:59")`, otherwise they will be formatted as `"Sun Apr 29 2007 23:59:59"`.


## File uploads

> On success, an HTTP 200 OK with JSON data of file data is returned:

```shell
$ curl https://todoist.com/API/v6/upload_file -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d file_name=example.jpg \
    --data-binary @example.jpg
{
  "file_name": "example.jpg",
  "file_size": 85665,
  "file_type": "image/jpeg",
  "file_url": "https://*.cloudfront.net/*/example.jpg",
  "tn_l": [
    "https://*.cloudfront.net/tn_l_*.jpg",
    400, 309
  ],
  "tn_m": [
    "https://*.cloudfront.net/tn_m_*.jpg",
    288, 222
  ],
  "tn_s": [
    "https://*.cloudfront.net/tn_s_*.jpg",
    96, 74
  ]
}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.upload_file('example.jpg')
{
  'file_name': 'example.jpg',
  'file_size': 85665,
  'file_type': 'image/jpeg',
  'file_url': 'https://*.cloudfront.net/*/example.jpg',
  'tn_l': [
    'https://*.cloudfront.net/tn_l_*.jpg',
    400, 309
  ],
  'tn_m': [
    'https://*.cloudfront.net/tn_m_*.jpg',
    288, 222
  ],
  'tn_s': [
    'https://*.cloudfront.net/tn_s_*.jpg',
    96, 74
  ]
}

```
Upload a file suitable to be passed as a `file_attachment` attribute to the `note_add` or `note_update` calls.

### Required parameters

Parameter | Description
--------- | -----------
token | The user's token (received on login).
file_name | The file name to be uploaded.

### Base file properties

Attribute | Description
--------- | -----------
file_name | The name of the file
file_size | The size of the file in bytes
file_type | MIME type
file_url | The URL where the file is located. Note that we don't cache the remote content on our servers and stream or expose files directly from third party resources. In particular this means that you should avoid providing links to non-encrypted (plain HTTP) respources, as exposing this files in Todoist may issue a browser warning.
upload_state | Upload completion state

### Image file properties

If you upload an image, you may provide thumbnail paths to ensure Todoist handles them appropriately. Valid thumbnail information is a JSON array with URL, width in pixels, height in pixels. Ex.: `["http://example.com/img.jpg",400,300]`. "Canonical" thumbnails (ones we create by `upload_file` API call) have following sizes: `96x96`, `288x288`, `528x528`.

Attribute | Description
--------- | -----------
tn_l | Large thumbnail.
tn_m | Medium thumbnail.
tn_s | Small thumbnail.

### Audio file properties

If you upload an audio file, you may provide an extra attribute `file_duration` (duration of the audio file in seconds, which takes an integer value). In the web interface the file is rendered back with a `<audio>` tag, so you should make sure it's supported in current web browsers. See [supported media formats](https://developer.mozilla.org/en-US/docs/HTML/Supported_media_formats) for the reference.
