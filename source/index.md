---
title: Todoist API

language_tabs:
  - shell: CURL
  - python: PYTHON

search: true
---

<!--

The MIT License (MIT)

Copyright (c) 2014 Doist

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

--> 

# Introduction

Welcome to the official Todoist API documentation!  The Todoist API can be used to hook [Todoist](https://todoist.com) with other applications, as it makes it easy for your client to retrieve and sync your data.

The Todoist API is based on [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer), so you can use it with a tool like [cURL](http://curl.haxx.se), but language bindings also exist for [Python](https://www.python.org), so it is easier and faster to write your own scripts or applications interacting with Todoist.

The [source code](https://github.com/Doist/todoist-api) of this API documentation is also available.

## Libraries

### Python

> You can install the Todoist Python library from the PyPI with:

```
$ pip install todoist-python
```

At the moment there are language bindings for Python, in the form of the [Todoist Python API library](https://github.com/Doist/todoist-python).

A [PyPI package](https://pypi.python.org/pypi/todoist-python) has been also prepared in order to easily install the Todoist Python library in your system.

There is more detailed documentation speficically for the Todoist Python API library, and this [API reference](http://todoist-python.readthedocs.org/en/latest/) documentation can be also read online.

# User

A user in Todoist is a JSON object. The dates will be in the UTC timezone. Typically a user object will have the following properties:

> An example of a user object:

```shell
{
  "start_page": "overdue, 7 days",
  "is_premium": false,
  "sort_order": 0,
  "full_name": "Example User",
  "has_push_reminders": false,
  "timezone": "Europe\/Athens",
  "id": 1855589,
  "next_week": 1,
  "completed_count": 20,
  "tz_offset": ["+03:00", 3, 0, 1],
  "email": "me@xample.com",
  "karma": 684.0,
  "start_day": 1,
  "date_format": 0,
  "inbox_project": 128501411,
  "time_format": 0,
  "image_id": null,
  "beta": 0,
  "karma_trend": "-",
  "business_account_id": null,
  "mobile_number": null,
  "mobile_host": null,
  "is_dummy": 0,
  "premium_until": null,
  "join_date": "Wed 30 Apr 2014 13:24:38 +0000",
  "token": "0123456789abcdef0123456789abcdef01234567",
  "is_biz_admin": false,
  "default_reminder": null
}
```

```python
{
  'beta': 0,
  'business_account_id': None,
  'completed_count': 20,
  'date_format': 0,
  'default_reminder': None,
  'email': 'me@exampe.com',
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
}
```
### Properties

Property | Description
-------- | -----------
id| User's unique id.
api_token | User's token (that should be used to call the other API methods).
email | User's email.
full_name | User's real name.
start_page | User's default view on Todoist. The start page can be one of the following: `_info_page` for the info page, `_blank` for a blank page, `_project_<PROJECT_ID>` for project with id `<PROJECT_ID>`, and `<ANY_QUERY>` to query after anything.
timezone | User's timezone in a string.
tz_offset | User's timezone offset `[GMT_STRING, HOURS, MINUTES, IS_DAYLIGHT_SAVINGS_TIME]`.
time_format | If it's `0` then show time as `13:00`, else show time as `1:00pm`.
date_format | If it's `0` then show dates as `DD-MM-YYYY`, else show dates as `MM-DD-YYYY`.
sort_order | If it's `0` then show `Oldest dates first` when viewing projects. Else `Oldest dates last`.
mobile_number | User's mobile number.
mobile_host | User's mobile host.
premium_until | When does the user's Premium subscription expire?
default_reminder | What is the default reminder for the user? Reminders are only possible for Premium users. The default reminder can be one of the following:  `email`, `mobile`, `push`, and `no_default`.

## Login

> On success, an HTTP 200 OK with a JSON object with user data is returned:

```shell
$ curl https://todoist.com/API/login \
    -d email=me@example.com \
    -d password=secret
{
  "start_page": "overdue, 7 days",
  "date_format": 0,
  "last_used_ip": "10.20.30.40",
  "api_token": "0123456789abcdef0123456789abcdef01234567",
  "karma_trend": "-",
  "inbox_project": 128501411,
  "time_format": 0,
  "image_id": null,
  "beta": 0,
  "sort_order": 0,
  "business_account_id": null,
  "full_name": "Example User",
  "mobile_number": null,
  "shard_id": 2,
  "timezone": "Europe/Athens",
  "is_premium": false,
  "mobile_host": null,
  "id": 1855589,
  "has_push_reminders": false,
  "is_dummy": 0,
  "premium_until": null,
  "team_inbox": null,
  "next_week": 1,
  "token": "0123456789abcdef0123456789abcdef01234567",
  "tz_offset": ["+03:00", 3, 0, 1],
  "join_date": "Wed 30 Apr 2014 13:24:38 +0000",
  "seq_no": 2180270834,
  "karma": 684.0,
  "is_biz_admin": false,
  "default_reminder": null,
  "email": "me@example.com"
}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI()
>>> api.login('me@example.com', 'secret')
{
  'api_token': '0123456789abcdef0123456789abcdef01234567',
  'beta': 0,
  'business_account_id': None,
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
  'last_used_ip': '10.20.30.40',
  'mobile_host': None,
  'mobile_number': None,
  'next_week': 1,
  'premium_until': None,
  'seq_no': 2180270834,
  'shard_id': 2,
  'sort_order': 0,
  'start_day': 1,
  'start_page': 'overdue, 7 days',
  'team_inbox': None,
  'time_format': 0,
  'timezone': 'Europe/Athens',
  'token': '0123456789abcdef0123456789abcdef01234567',
  'tz_offset': ['+03:00', 3, 0, 1]
}
```

Login user into Todoist to get a token, which is necessary to do any communication with the server.

### Required parameters

Parameter | Description
--------- | -----------
email | User's email.
password | User's password.

### Error returns

Error | Description
----- | -----------
LOGIN_ERROR | Raises when the token is invalid or outdated (Google refuses to accept this token.)
INTERNAL_ERROR | If server is unable to connect with Google to check the token or if the response from Google is unable to parse. It makes sense to repeat the attempt with same set of options later.
EMAIL_MISMATCH | The token is valid, but the email accompanied the token in the request mismatches the one returned by Google.
ACCOUNT_ NOT_CONNECTED_ WITH_GOOGLE | Raises when token is valid and matches the email, but Todoist account is not connected with Google.

## Login with Google

> On success, an HTTP 200 OK with a JSON object with user data is returned:

```shell
$ curl https://todoist.com/API/loginWithGoogle \
    -d email=me@example.com \
    -d oauth2_token=01234567-89ab-cdef-0123-456789abcdef

{
  "start_page": "overdue, 7 days",
  "date_format": 0,
  "last_used_ip": "10.20.30.40",
  "api_token": "0123456789abcdef0123456789abcdef01234567",
  "karma_trend": "-",
  "inbox_project": 128501411,
  "time_format": 0,
  "image_id": null,
  "beta": 0,
  "sort_order": 0,
  "business_account_id": null,
  "full_name": "Example User",
  "mobile_number": null,
  "shard_id": 2,
  "timezone": "Europe/Athens",
  "is_premium": false,
  "mobile_host": null,
  "id": 1855589,
  "has_push_reminders": false,
  "is_dummy": 0,
  "premium_until": null,
  "team_inbox": null,
  "next_week": 1,
  "token": "0123456789abcdef0123456789abcdef01234567",
  "tz_offset": ["+03:00", 3, 0, 1],
  "join_date": "Wed 30 Apr 2014 13:24:38 +0000",
  "seq_no": 2180270834,
  "karma": 684.0,
  "is_biz_admin": false,
  "default_reminder": null,
  "email": "me@example.com"
}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI()
>>> api.login_with_google('me@example.com', '01234567-89ab-cdef-0123-456789abcdef')
{
  'api_token': '0123456789abcdef0123456789abcdef01234567',
  'beta': 0,
  'business_account_id': None,
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
  'last_used_ip': '10.20.30.40',
  'mobile_host': None,
  'mobile_number': None,
  'next_week': 1,
  'premium_until': None,
  'seq_no': 2180270834,
  'shard_id': 2,
  'sort_order': 0,
  'start_day': 1,
  'start_page': 'overdue, 7 days',
  'team_inbox': None,
  'time_format': 0,
  'timezone': 'Europe/Athens',
  'token': '0123456789abcdef0123456789abcdef01234567',
  'tz_offset': ['+03:00', 3, 0, 1]
}
```

Login user into Todoist with Google account. Can be used instead of normal login to get a token.

### Implementation notes

While authenticating, application exchanges their users' OAuth 2.0 access tokens to tokens provided by Todoist. OAuth 2.0 access token must be issued for the scope `oauth2:https://www.googleapis.com/auth/userinfo.email`

It doesn't matter how you receive the access token. Android applications can use [AccountManager](http://developer.android.com/reference/android/accounts/AccountManager.html) to get it done. Other developers should consider using [generic authentication instructions](https://developers.google.com/accounts/docs/OAuth2Login).

### Required parameters

Parameter | Description
--------- | -----------
email | User's email.
oauth2_token | OAuth 2.0 access token, received by application from user.

### Optional parameters

Parameter | Description
--------- | -----------
auto_signup | If set to `1` and user with this email and Google Account doesn't exist yet, then automatically registers a new account.
full_name | User's full name if user is about to be registered. If not set, a user email nickname will be used.
timezone | User's timezone if user is about to be registered. If not set, we guess the timezone from the client's IP address. In case of failure, "UTC" timezone will be set up for a newly created account.
lang | User's language. Can be `de`, `fr`, `ja`, `pl`, `pt_BR`, `zh_CN`, `es`, `hi`, `ko`, `pt`, `ru`, `zh_TW`

### Error returns

Error | Description
----- | -----------
LOGIN_ERROR | Raises when the token is invalid or outdated (Google refuses to accept this token.)
INTERNAL_ERROR | If server is unable to connect with Google to check the token or if the response from Google is unable to parse. It makes sense to repeat the attempt with same set of options later.
EMAIL_MISMATCH | The token is valid, but the email accompanied the token in the request mismatches the one returned by Google.
ACCOUNT_ NOT_CONNECTED_ WITH_GOOGLE | Raises when token is valid and matches the email, but Todoist account is not connected with Google.

## Test token

> On success, HTTP 200 OK with text "ok" is returned:

```shell
$ curl https://todoist.com/API/ping \
    -d token=0123456789abcdef0123456789abcdef01234567
ok
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI()
>>> api.ping('0123456789abcdef0123456789abcdef01234567')
'ok'
```

Test user's login token.

### Required parameters

Parameter | Description
--------- | -----------
token | User's login token.

### Error returns

Error | Description
----- | -----------
HTTP 401 | Token not correct!

## Get timezones

> On success, HTTP 200 OK with a JSON object with timezone names is returned:

```shell
$ curl https://todoist.com/API/getTimezones
[
  ["US\/Hawaii", "(GMT-1000) Hawaii"],
  ["US\/Alaska", "(GMT-0800) Alaska"],
  ["US\/Pacific", "(GMT-0700) Pacific Time (US & Canada)"],
  ...
]
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI()
>>> api.getTimezones()
[
  ['US/Hawaii', '(GMT-1000) Hawaii'],
  ['US/Alaska', '(GMT-0800) Alaska'],
  ['US/Pacific', '(GMT-0700) Pacific Time (US & Canada)'],
  ...
]
```

Returns the timezones Todoist supports.

## Update properties


> An example of updating the user's properties:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "user_update", "timestamp": 1411655847093, "args": {"time_format": 0}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.user_update(time_format=0)
```

Update the details of the user.

### Required parameters

Parameter | Description
--------- | -----------
token | The user's token (received on login).

### Optional parameters

Parameter | Description
--------- | -----------
email | User's email.
full_name | User's full name.
password | User's password, which should be at least 5 characters long.
timezone | User's timezone (check the `getTimezones` request).
date_format | How should dates be formatted? If `0` `DD-MM-YYYY` will be used, if `1` `MM-DD-YYYY` will be used.
time_format | How should times be formatted? If `0` `13:00` will be used, if `1` `1:00pm` will be used.
start_day | First day of week. `1` for Monday, `7` for Sunday.
next_week | When postponing what day should we choose? `1` for Monday, `7` for Sunday.
start_page | Can be one of following values: `_blank` to show blank page, `_info_page` to show info page `_project_<PROJECT_ID>` where `<PROJECT_ID>` is the id of the project to show, `<ANY_QUERY>` to query after anything (for example `tod,tom,!!1`).
default_reminder | Can be one of the following values: `email` to send reminders by email, `mobile` to send reminders to mobile devices via SMS, `push` to send reminders to smart devices using push notifications (one of Android or iOS official client must be installed on the client side to receive these notifications), `no_default` to turn off sending default reminders.

# Data

## Recommended workflow

This is the recommend way to work with the API.

Creating and updating the full model (on login or relaunch of your app):

![get](/images/get.png)

Updating changes and getting updated data (iterative sync):

![get](/images/sync-get.png)

## Retrieve data

> An example of the JSON object returned, of a new user's empty account complete data:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/get -X POST \
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

The `get` API call is used to retrieve data (both all and only updated).

### Required parameters

Parameter | Description
--------- | -----------
seq_no | Sequence number. On the initial request you should pass `0`. On all other requests you should pass the last `seq_no` you received from the server.

### Optional parameters

Parameter | Description
--------- | -----------
day_orders_timestamp | The `get` and `syncAndGetUpdated` API requests return `DayOrdersTimestamp` that specifies when the day orders were last updated. If you omit `day_orders_timestamp` then all the day orders will be fetched by the `get` API request and none of then will be fetched by `syncAndGetUpdated` request. If you specify `day_orders_timestamp` then day orders will be returned if your timestamp is different from the servers. If you send `day_orders_timestamp` and the day orders have not been updated then the server won't return the `DayOrders` entry at all.
include_notification_settings | Include notification settings (`SettingsNotifications`), the same data as `getNotificationSettings` API request returns. This is needed on platforms that implement native notifications.
resource_types | An optional parameter which is useful if you don't need a complete result of `get` API request and `syncAndGetUpdated`, but want to have just a subset. It can be useful for speeding up the load of most important user's data (like the list of projects and tasks) and to get the rest of the data asynchronously later on. It should be a JSON-encoded list of strings. For example, `["projects", "labels", "filters"]`. Below is the list of recognizable values for strings: `projects` for the list of projects and tasks, `labels` for the list of labels, `day_orders` for the list of day orders, `filters` for the list of filters, `reminders` for the list of reminders,  `collaborators` for the list of collaborators, `notes` for the list of notes, `live_notifications` for the list of live notifications, `share_invitations` for the list of invitations
api_token | User's API token (returned on successful login). Else the session cookie is used.

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
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "project_add", "temp_id": "$1411653993012", "timestamp": 1411653993012, "args": {"name": "Project1", "item_order": 1, "indent": 1, "color": 1}}]'
{"$1411653993012": 128501470}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.sync(items_to_sync=[{'type': 'project_add', 'temp_id': '$1411653993012', 'timestamp': 1411653993012, 'args': {'name': 'Project1', 'item_order': 1, 'indent': 1, 'color': 1}}]
{'$1411653993012': 128501470}
```

The `sync` API call takes a list of JSON object commands which specify which changes should be done to the model. These changes could be adding projects, deleting tasks, adding notes etc.

It supports two big features which can make syncing easier: temporary ids, that is the ability to use and resolve temporary ids, and duplication protection which ensures that a command isn't run two times.

### Required parameters

Parameter | Description
--------- | -----------
items_to_sync | A list of JSON object commands. These commands are specified later in the documentation.
api_token | User's API token (returned on successful login). Else the session cookie is used.

## Send and retrieve data

> Example of creating a new project, where an HTTP 200 OK with a JSON object with the new data since the last change of temporary id to real id mappings is returned (this will be explained later):

```shell
$ curl https://todoist.com/TodoistSync/v5.3/syncAndGetUpdated -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d seq_no=2180586309
    -d items_to_sync='[{"type": "project_add", "temp_id": "$1411654079274", "timestamp": 1411654079274, "args": {"name": "Project2", "item_order": 1, "indent": 1, "color": 1}}]'
{
  "seq_no_global": 2180600512,
  "Collaborators": [],
  "DayOrdersTimestamp": "1344642991.1",
  "Notes": [],
  "Labels": [],
  "UserId": 1855589,
  "CollaboratorStates": [],
  "LiveNotifications": [],
  "seq_no": 2180600512,
  "Filters": [],
  "Items": [],
  "TempIdMapping": {"$1411654079274": 128501607},
  "Reminders": [],
  "Projects": [],
  "LiveNotificationsLastRead": 0
}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.get()
>>> api.sync_and_get_updated(items_to_sync=[{'type': 'project_add', 'temp_id': '$1411654079274', 'timestamp': 1411654079274, 'args': {'name': 'Project2', 'item_order': 1, 'indent': 1, 'color': 1}}]
{
  'seq_no_global': 2180600512L,
  'Collaborators': [],
  'DayOrdersTimestamp': '1344642991.1',
  'Notes': [],
  'Labels': [],
  'UserId': 1855589,
  'CollaboratorStates': [],
  'LiveNotifications': [],
  'Filters': [],
  'Items': [],
  'TempIdMapping': {'$1411654079274': 128501607},
  'LiveNotificationsLastRead': 0,
  'Reminders': [],
  'Projects': [],
  'seq_no': 2180600512L
}
```

The `syncAndGetUpdated` API call is a combined call that syncs updates and fetches the latest data. This is useful because you don't have to do two requests.

### Required parameters

Parameter | Description
--------- | -----------
api_token | User's API token (returned on successful login). Else the session cookie is used.

### Optional parameters

Parameter | Description
--------- | -----------
items_to_sync | A list of JSON object commands. It's the items_to_sync parameter of `sync` API call.
seq_no | Sequence number. On the initial request you should pass 0. On all other requests you should pass the last seq_no you recieved from the server.
day_orders_timestamp | A string value specifying when day_orders where last updated. It's the `DayOrdersTimestamp` parameter of `get` API call. Please see that documentation for further details.
include_notification_settings | Include notification settings (`SettingsNotifications`), the same data as the `getNotificationSettings` request  returns. This is needed on platforms that implement native notifications.

## Timestamps

Each `timestamp` should be a unix timestamp of when the command was created. This is used for duplication protection, i.e. we want to ensure that same command isn't executed twice! This check works by keeping track of command types, temporary ids and timestamps. By just supplying timestamps the system does these checks automatically and will ignore duplicated commands!

## Temporary ids

> An example that shows how temporary ids can be used and referenced:

```json
[
  { "type": "project_add",
    "temp_id": "$1411654161623",
    "args": {
      "name": "Project3",
      "item_order": 1,
      "indent": 1,
      "color": 1
    },
    "timestamp": 1411654161623
  },
  {
    "type": "project_update",
    "args": {
      "id": "$1411654161623",
      "color": 2
    },
    "timestamp": 1411654193536
  }
]
```

>  You can see that the project_add command has a temp_id property, which specifies which temporary id this new project has. The project_update command later references this temporary id. The API will automatically resolve these ids. Additionally `sync` will return the real id in the result. Remember to update your local model with these real ids e.g.:

```json
{"$1411654161623": 128501682}
```

Your application will use temporary ids and the Sync API has special support for them. We suggest that you use unix timestamps to generate these ids to give them uniqueness (maybe also prefixing them with a special number).

While the system remembers temporary ids and their mappings to real ids, it's important to use real ids when they are available to you (typically after a sync). This is important since the API only remembers the last 500 temporary ids for each user!

## Date query and search

> On success, an HTTP 200 OK with a JSON object with the tasks found is returned:

```shell
$ curl https://todoist.com/API/query \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d queries='["tomorrow","p1"]'
[
  {
    "query": "tomorrow",
    "type": "date",
    "data": [
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
    u'data': [
      { u'assigned_by_uid': 1855589,
        u'checked': 0,
        u'children': None,
        u'collapsed': 0,
        u'complete_count': 0,
        u'content': u'Task2',
        u'date_added': u'Tue 07 Oct 2014 06:11:06 +0000',
        u'date_string': u'8 Oct',
        u'day_order': 0,
        u'due_date': u'Wed 08 Oct 2014 20:59:59 +0000',
        u'has_notifications': 0,
        u'id': 35825425,
        u'in_history': 0,
        u'indent': 1,
        u'is_archived': 0,
        u'is_deleted': 0,
        u'is_dst': None,
        u'item_order': 2,
        u'labels': [],
        u'mm_offset': 180,
        u'postpone_count': 0,
        u'priority': 1,
        u'project_id': 128501470,
        u'responsible_uid': None,
        u'seq_no': 2306453653L,
        u'sync_id': None,
        u'user_id': 1855589 },
      { u'assigned_by_uid': 1855589,
        u'checked': 0,
        u'children': None,
        u'collapsed': 0,
        u'complete_count': 0,
        u'content': u'Task 3',
        u'date_added': u'Tue 07 Oct 2014 06:20:48 +0000',
        u'date_string': u'8 Oct',
        u'day_order': 1,
        u'due_date': u'Wed 08 Oct 2014 20:59:59 +0000',
        u'has_notifications': 0,
        u'id': 35826737,
        u'in_history': 0,
        u'indent': 1,
        u'is_archived': 0,
        u'is_deleted': 0,
        u'is_dst': None,
        u'item_order': 3,
        u'labels': [],
        u'mm_offset': 180,
        u'postpone_count': 0,
        u'priority': 1,
        u'project_id': 128501470,
        u'responsible_uid': None,
        u'seq_no': 2306541514L,
        u'sync_id': None,
        u'user_id': 1855589 }
    ],
    u'query': u'tomorrow',
    u'type': u'date'
  },
  {
    u'data': [
      { u'assigned_by_uid': 1855589,
        u'checked': 0,
        u'children': None,
        u'collapsed': 0,
        u'complete_count': 1,
        u'content': u'Task1',
        u'date_added': u'Fri 26 Sep 2014 11:54:48 +0000',
        u'date_string': u'',
        u'due_date': None,
        u'has_notifications': 0,
        u'id': 33548400,
        u'in_history': 0,
        u'indent': 1,
        u'is_archived': 0,
        u'is_deleted': 0,
        u'is_dst': None,
        u'item_order': 1,
        u'labels': [],
        u'mm_offset': 180,
        u'postpone_count': 5,
        u'priority': 4,
        u'project_id': 128501470,
        u'responsible_uid': None,
        u'seq_no': 2306454606L,
        u'sync_id': None,
        u'user_id': 1855589}],
    u'query': u'p1',
    u'type': u'priority'
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
$ curl https://todoist.com/API/uploadFile -X POST \
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

### Base file properties

Attribute | Description
--------- | -----------
file_name | The name of the file
file_size | The size of the file in bytes
file_type | MIME type
file_url | The URL where the file is located. Note that we don't cache the remote content on our servers and stream or expose files directly from third party resources. In particular this means that you should avoid providing links to non-encrypted (plain HTTP) respources, as exposing this files in Todoist may issue a browser warning.
upload_state | Upload completion state

### Image file properties

If you upload an image, you may provide thumbnail paths to ensure Todoist handles them appropriately. Valid thumbnail information is a JSON array with URL, width in pixels, height in pixels. Ex.: `["http://example.com/img.jpg",400,300]`. "Canonical" thumbnails (ones we create by `uploadFile` API call) have following sizes: `96x96`, `288x288`, `528x528`.

Attribute | Description
--------- | -----------
tn_l | Large thumbnail.
tn_m | Medium thumbnail.
tn_s | Small thumbnail.

### Audio file properties

If you upload an audio file, you may provide an extra attribute `file_duration` (duration of the audio file in seconds, which takes an integer value). In the web interface the file is rendered back with a `<audio>` tag, so you should make sure it's supported in current web browsers. See [supported media formats](https://developer.mozilla.org/en-US/docs/HTML/Supported_media_formats) for the reference.

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
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "project_add", "temp_id": "$1411654292446", "timestamp": 1411654292446, "args": {"name": "Project4"}}]'
{'$1411654292446': 128501815}
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
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync=[{"type": "project_update", "timestamp": 1411654327479, "args": {"id": 128501815, "indent": 2}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> project = api.project_get_by_id(128501815)
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

$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync=[{"type": "project_delete", "timestamp": 1411654352338, "args": {"ids": [128501815]}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> project = api.project_get_by_name('Project4')
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
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync=[{"type": "project_archive", "timestamp": 1411654438975, "args": {"id": 128501682}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> project = api.project_get_by_id(128501682)
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
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync=[{"type": "project_unarchive", "timestamp": 1411654459062, "args": {"id": 128501682}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> project = api.project_get_by_name('Project4')
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
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync=[{"type": "project_update_orders_indents", "timestamp": 1411654466656, "args": {"ids_to_orders_indents": {"128501470": [42, 1], "128501607": [43, 1]}}}]'
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
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "item_add", "temp_id": "$1411732480502", "timestamp": 1411732480502, "args": {"content": "Task1", "project_id": 128501470}}]'
{"$1411732480502": 33548400}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> item = api.item_add('Task1', 128501470)
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
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "item_update", "timestamp": 1411984521047, "args": {"id": 33548400, "priority": 2}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> item = api.item_get_by_id(33548400)
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
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "item_delete", "timestamp": 1411984533965, "args": {"ids": [33548400]}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> item = api.item_get_by_content('Task1')
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
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "item_move", "timestamp": 1411997433394, "args": {"project_items": {"128501470": [33548400]}, "to_project": 128501607}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.item_get_by_id(33548400)
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
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "item_complete", "timestamp": 1412001010042, "args": {"project_id": 128501470, "ids": [33548400]}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.item_get_by_content('Task1')
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
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "item_uncomplete", "timestamp": 1412001918951, "args": {"project_id": 128501470, "ids": [33548400]}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.item_get_by_id(33548400)
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
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "item_uncomplete_update_meta", "timestamp": 1412061611473, "args": {"project_id": 128501470, "ids_to_metas": {"33548400": [0, 0, 1]}}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.item_uncomplete_update_meta(128501470, {33548400: [0, 0, 1]})
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
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "item_update_date_complete", "timestamp": 1412089326280, "args": {"id": 33548400, "new_data_utc": "2014-10-30T23:59", "date_string": "every day", "is_forward": 1}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.item_update_date_complete(33548400, '2014-10-30T23:59', 'every day', 1)
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
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "item_update_orders_indents", "timestamp": 1412089328586, "args": {"ids_to_orders_indents": {"33548400": [1, 1]}}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.item_update_orders_indents({33548400: [1, 1]})
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
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "item_update_day_orders", "timestamp": 1412089509020, "args": {"ids_to_orders": {"33548400": 1}}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.item_update_day_orders({33548400: 1})
>>> api.commit()
```

Update the day orders of multiple items at once.

### Required arguments

Argument | Description
-------- | -----------
ids_to_orders | A dictionary, where an item id is the key, and the day order its value: `item_id: day_order`.

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

# Notes

> A note in Todoist is a JSON object. Typically a note object will look like:

```shell
{
  "is_deleted": 0,
  "is_archived": 0,
  "content": "Note",
  "file_attachment": {
    "file_type": "text/plain",
    "file_name": "File1.txt",
    "file_size": 1234,
    "file_url": "https://example.org/File1.txt",
    "upload_state": "completed"
  }
  "posted_uid": 1855589,
  "item_id": 33548400,
  "uids_to_notify": null,
  "id": 1234,
  "posted": "Wed 01 Oct 2014 14:54:55 +0000"
}
```

```python
{
  'is_deleted': 0,
  'is_archived': 0,
  'content': 'Note',
  'item_id': 33548400,
  'posted_uid': 1855589,
  'posted': 'Wed 01 Oct 2014 14:54:55 +0000',
  'id': 1234,
  'file_attachment': {
    'file_type': 'text/plain',
    'file_name': 'File1.txt',
    'file_size': 1234,
    'file_url': 'https://example.org/File1.txt',
    'upload_state': 'completed'
  }
  'uids_to_notify': None
}
```

### Properties

Property | Description
-------- | -----------
id | The id of the note.
content | The content of the note.
item_id | The item which the note is part of.
file_attachment | A file attached to the note.

### File attachments

A file attachment is represented as a JSON object. The file attachment may point to a document, previously uploaded by the `uploadFile` API call, or by any external resource.

### Base file properties

Attribute | Description
--------- | -----------
file_name | The name of the file.
file_size | The size of the file in bytes.
file_type | MIME type.
file_url | The URL where the file is located. Note that we don't cache the remote content on our servers and stream or expose files directly from third party resources. In particular this means that you should avoid providing links to non-encrypted (plain HTTP) respources, as exposing this files in Todoist may issue a browser warning.
upload_state | Upload completion state.

### Image file properties

If you upload an image, you may provide thumbnail paths to ensure Todoist handles them appropriately. Valid thumbnail information is a JSON array with URL, width in pixels, height in pixels. Ex.: ["http://example.com/img.jpg",400,300]. "Canonical" thumbnails (ones we create by `uploadFile` API call) have following sizes: 96x96, 288x288, 528x528.

Attribute | Description
--------- | -----------
tn_l | Large thumbnail.
tn_m | Medium thumbnail.
tn_s | Small thumbnail.

### Audio file properties

If you upload an audio file, you may provide an extra attribute `file_duration` (duration of the audio file in seconds, which takes an integer value). In the web interface the file is rendered back with a `<audio>` tag, so you should make sure it's supported in current web browsers. See [supported media formats](https://developer.mozilla.org/en-US/docs/HTML/Supported_media_formats) for the reference.


## Add a note

> An example of adding a note:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "note_add", "temp_id": "$1412325057551", "timestamp": 1412325057551, "args": {"item_id": 33548400, "content": "Note1"}}]'
{"$1412325057551": 1234}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> note = api.note_add(33548400, 'Note1')
>>> api.commit()
```

Add a note.

### Required arguments

Argument | Description
-------- | -----------
item_id | The id of the item
content | The content of the note.

### Optional arguments

Argument | Description
-------- | -----------
file_attachment | A file attached to the note.

## Update a note

> An example of updating a note:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "note_update", "timestamp": 1412325418683, "args": {"id": 1234, "content": "UpdatedNote1"}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> note api.note_get_by_id(1234)
>>> note.update(content='UpdatedNote1')
>>> api.commit()
```

Update a note.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the note.

### Optional arguments

Argument | Description
-------- | -----------
content | The content of the note.
file_attachment | A file attached to the note.

## Delete a note

> An example of deleting a note:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "note_delete", "timestamp": 1412325418683, "args": {"id": 1234}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> note = api.note_get_by_id(1234)
>>> note.delete()
>>> api.commit()
```

Delete a note.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the note.

# Filters

> A filter in Todoist is a JSON object. Typically a filter object will look like:

```shell
{
  "user_id": 1855589,
  "name": "Priority 1",
  "color": 6,
  "item_order": 3,
  "query": "priority 1",
  "is_deleted": 0,
  "id": 4638878
}
```

```python
{
  'user_id': 1855589,
  'name': 'Priority 1',
  'color': 6,
  'is_deleted': 0,
  'item_order': 3,
  'query': 'priority 1',
  'id': 4638878
}
```

### Properties

Property | Description
-------- | -----------
id | The id of the filter.
name | The name of the filter.
item_order | Filter's order in the filter list.
color | The color of the filter.
query | The query to search for. [Examples of searches](https://todoist.com/Help/timeQuery) can be found in the Todoist help page.

## Add a filter

> An example of adding a filter:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "filter_add", "temp_id": "$1412333089053", "timestamp": 1412333089053, "args": {"name": "Filter1", "query": "no due date"}}]'
{"$1412333089053": 9}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> filter = api.filter_add('Filter1', 'no due date')
>>> api.commit()
```

Add a filter.

### Required arguments

Argument | Description
-------- | -----------
name | The name of the filter.
query | The query to search for. [Examples of searches](https://todoist.com/Help/timeQuery) can be found in the Todoist help page.

### Optional arguments

Argument | Description
-------- | -----------
item_order | Filter's order in the filter list.
color | The color of the filter.

## Update a filter

> An example of updating a filter:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "filter_update", "timestamp": 1412334986045, "args": {"id": 9, "query": "tomorrow"}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> filter = api.filter_get_by_id(9)
>>> filter.update(query='tomorrow')
>>> api.commit()
```

Update a filter.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the filter.

### Optional arguments

Argument | Description
-------- | -----------
query | The query to search for. [Examples of searches](https://todoist.com/Help/timeQuery) can be found in the Todoist help page.
item_order | Filter's order in the filter list.
color | The color of the filter.

## Delete a filter

> An example of deleting a filter:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "filter_delete", "timestamp": 1412336184893, "args": {"id": 9}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> filter = api.filter_get_by_id(9)
>>> filter.delete()
>>> api.commit()
```

Delete a filter.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the filter.

## Update multiple orders

> An example of updating the orders of multiple filters at once:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync=[{"type": "filter_update_orders", "timestamp": 1412336529401, "args": {"id_order_mapping": {"9":  1], "10": 2]}}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.filter_update_orders({9: 1, 10: 2})
>>> api.commit()
```

### Required arguments

Argument | Description
-------- | -----------
id_order_mapping| A dictionary, where a filter id is the key, and the order its value: `filter_id: order`.

# Reminders

> A reminder in Todoist is a JSON object. Typically a reminder object will look like:

```shell
{
  "due_date": "Mon 06 Oct 2014 11:00:00 +0000",
  "due_date_utc": "Mon 06 Oct 2014 11:00:00 +0000",
  "is_deleted": 0,
  "service": "email",
  "item_id": 33511505,
  "mm_offset": 180,
  "notify_uid": 1855589,
  "type": "absolute",
  "id": 1234,
  "date_string": "Oct 6 @ 2pm"
}
```

```python
{
  'due_date': 'Mon 06 Oct 2014 11:00:00 +0000',
  'is_deleted': 0,
  'service': 'email',
  'mm_offset': 180,
  'due_date_utc': 'Mon 06 Oct 2014 11:00:00 +0000',
  'item_id': 33511505,
  'notify_uid': 1855589,
  'type': 'absolute',
  'id': 1234,
  'date_string': 'Oct 6 @ 2pm'
}
```

### Properties

Property | Description
-------- | -----------
id | The id of the reminder.
item_id | The item id for which the reminder is about.
service | The way to get notified of the reminder: `email` for e-mail, `sms` for mobile text message, or `push` for mobile push notification.
type | The type of the reminder: `relative` for a time-based reminder specified in minutes from now, `absolute` for a time-based reminder with a specific time and date in the future, and `location` for a location-based reminder.
due_date_utc | Should be formatted as `YYYY-MM-DDTHH:MM`, example: `2012-3-24T23:59`. Value of `due_date_utc` must be in UTC. If you want to pass in due dates, note that `date_string` is required, while `due_date_utc` (`due_date`) can be omitted. If date_string is provided, it will be parsed as local timestamp, and converted to UTC internally, according to the user's profile settings.
date_string | The date of the task, added in free form text, for example it can be `every day @ 10`. Look at our reference to see [which formats are supported](https://todoist.com/Help/timeInsert).
notify_uid | The user id which should be notified of the reminder, typically the current user id creating the reminder.

## Add a reminder

> An example of adding a relative reminder:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "reminder_add", "temp_id": "$1412573453481", "timestamp": 1412573453481, "args": {"item_id": 33511505, "service": "email", "minute_offset": 30}}]'
{"$1412573453481": 1234}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> reminder = api.reminder_add(33511505, service='email', minute_offset=30)
>>> api.commit()
```

> An example of adding an absolute reminder:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "reminder_add", "temp_id": "$1412573453481", "timestamp": 1412573453481, "args": {"item_id": 33511505, "service": "email", "due_date_utc": "2014-10-15T11:00"}}]'
{"$1412573453481": 1234}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> reminder = api.reminder_add(33511505, service='email', due_date_utc='2014-10-15T11:00')
>>> api.commit()
```

> An example of adding a location reminder:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "reminder_add", "temp_id": "$1412573453481", "timestamp": 1412573453481, "args": {"item_id": 33511505, "service": "email", "type": "location", "name": "Aliados", "loc_lat": "41.148581", "loc_long":"-8.610945000000015", "loc_trigger":"on_enter", "radius": 100}}]'
{"$1412573453481": 1234}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> reminder = api.reminder_add(33511505, service='email', type='location', name='Aliados', loc_lat='41.148581', loc_long='-8.610945000000015', loc_trigger='on_enter', radius=100)
>>> api.commit()
```

Add a reminder.

### Required arguments

Argument | Description
-------- | -----------
item_id | The item id for which the reminder is about.

### Optional arguments

Argument | Description
-------- | -----------
service | The way to get notified of the reminder: `email` for e-mail, `sms` for mobile text message, or `push` for mobile push notification.
type | The type of the reminder: `relative` for a time-based reminder specified in minutes from now, `absolute` for a time-based reminder with a specific time and date in the future, and `location` for a location-based reminder.
minute_offset | The relative time in minutes before the due date of the item, in which the reminder should be triggered. Note, that the item should have a due date set in order to add a relative reminder.
due_date_utc | Should be formatted as `YYYY-MM-DDTHH:MM`, example: `2012-3-24T23:59`. Value of `due_date_utc` must be in UTC. If you want to pass in due dates, note that `date_string` is required, while `due_date_utc` (`due_date`) can be omitted. If date_string is provided, it will be parsed as local timestamp, and converted to UTC internally, according to the user's profile settings.
date_string | The date of the task, added in free form text, for example it can be `every day @ 10`. Look at our reference to see [which formats are supported](https://todoist.com/Help/timeInsert).
notify_uid | The user id which should be notified of the reminder, typically the current user id creating the reminder.
name | An alias name for the location.
loc_lat | The location latitude.
loc_long | The location longitude.
loc_trigger | What should the trigger the reminder: `on_enter` for entering the location, or `on_leave` for leaving the location.
radius | The radius around the location that is still considered as part of the location.

## Update a reminder

> An example of updating a reminder:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "reminder_update", "timestamp": 1412581522129, "args": {"id": 1234, "due_date_utc": "2014-10-10T15:00"}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> reminder = api.reminder_get_by_id(1234)
>>> reminder.update(due_date_utc='2014-10-10T15:00')
>>> api.commit()
```

Update a reminder.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the filter.

### Optional arguments

Argument | Description
-------- | -----------
minute_offset | The relative time in minutes from the current time in which the reminder should be triggered.
due_date_utc | Should be formatted as `YYYY-MM-DDTHH:MM`, example: `2012-3-24T23:59`. Value of `due_date_utc` must be in UTC. If you want to pass in due dates, note that `date_string` is required, while `due_date_utc` (`due_date`) can be omitted. If date_string is provided, it will be parsed as local timestamp, and converted to UTC internally, according to the user's profile settings.
date_string | The date of the task, added in free form text, for example it can be `every day @ 10`. Look at our reference to see [which formats are supported](https://todoist.com/Help/timeInsert).
notify_uid | The user id which should be notified of the reminder, typically the current user id creating the reminder.
type | The type of the reminder: `relative` for a time-based reminder specified in minutes from now, `absolute` for a time-based reminder with a specific time and date in the future, and `location` for a location-based reminder.
name | An alias name for the location.
loc_lat | The location latitude.
loc_long | The location longitude.
loc_trigger | What should the trigger the reminder: `on_enter` for entering the location, or `on_leave` for leaving the location.
radius | The radius around the location that is still considered as part of the location.

## Delete a reminder

> An example of deleting a reminder:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "reminder_delete", "timestamp": 1412336184893, "args": {"id": 9}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> reminder = api.reminder_get_by_id(1234)
>>> reminder.delete()
>>> api.commit()
```

Delete a reminder.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the filter.

# Live notifications

## Mark last read

> An example of marking the last read notification:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "live_notifications_mark_as_read", "timestamp": 1412581522129, "args": {"seq_no": 1234}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.live_notifications_mark_as_read(1234)
>>> api.commit()
```

Mark the last read live notification.

### Required arguments

Argument | Description
-------- | -----------
seq_no | The sequence number of the last read notification.

# Sharing

Commands that are related to sharing projects will be described in this section.

## Share a project

> An example of sharing a project:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "share_project", "temp_id": "$1412585639269", "timestamp": 1412585639269, "args": {"project_id": "128501470", "message": "", "email": "you@example.com"}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.share_project(128501470, 'you@example.com', message='')
>>> api.commit()
```

Share a project.

### Required arguments

Argument | Description
-------- | -----------
project_id | The project to be shared.
email | The user email with whom to share the project.

### Optional arguments

Argument | Description
-------- | -----------
message | A message to be sent to the user.

## Delete a collaborator

> An example of deleting a person from a shared project:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "delete_collaborator", "timestamp": 1412586651929, "args": {"project_id": 128501470, "email": "you@example.com"}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.delete_collaborator(128501470, 'you@example.com')
>>> api.commit()
```

Delete a person from a shared project.

### Required arguments

Argument | Description
-------- | -----------
project_id | The project to be shared.
email | The user email with whom the project was shared with.

## Accept an invitation

> An example of accepting an invitation:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "accept_invitation", "timestamp": 1412587467359, "args": {"invitation_id": 1234,  "invitation_secret": "abcdefghijklmno"}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.accept_invitation(1234, 'abcdefghijklmno')
>>> api.commit()
```

Accept an invitation.

### Required arguments

Argument | Description
-------- | -----------
invitation_id | The invitation id.
invitation_secret | The secret fetched from the live notification.

## Reject an invitation

> An example of rejecting an invitation:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "reject_invitation", "timestamp": 1412587669904, "args": {"invitation_id": 1234,  "invitation_secret": "abcdefghijklmno"}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.reject_invitation(1234, 'abcdefghijklmno')
>>> api.commit()
```

Reject an invitation.

### Required arguments

Argument | Description
-------- | -----------
invitation_id | The invitation id.
invitation_secret | The secret fetched from the live notification.

## Delete an invitation

> An example of deleting an invitation:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "delete_invitation", "timestamp": 1412587915563, "args": {"invitation_id": 128501470}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.delete_invitation(128501470)
>>> api.commit()
```

Delete an invitation.

### Required arguments

Argument | Description
-------- | -----------
invitation_id | The invitation to be deleted.

## Take ownership

> An example of taking ownership of a shared project:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "take_ownership", "timestamp": 1412588044477, "args": {"id": 128501470}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.take_ownership(128501470)
>>> api.commit()
```

Take ownership of a shared project.

### Required arguments

Argument | Description
-------- | -----------
id | The shared project of which to take the ownership.

## Accept a business invitation

> An example of accepting a business invitation:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "biz_accept_invitation", "timestamp": 1412588256845, "args": {"invitation_id": 1234,  "invitation_secret": "abcdefghijklmno"}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.biz_accept_invitation(1234, 'abcdefghijklmno')
>>> api.commit()
```

Accept an invitation from Todoist for Business.

### Required arguments

Argument | Description
-------- | -----------
invitation_id | The invitation id.
invitation_secret | The secret fetched from the live notification.

## Reject a business invitation

> An example of rejecting a business invitation:

```shell
$ curl https://todoist.com/TodoistSync/v5.3/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "biz_reject_invitation", "timestamp": 1412588264690, "args": {"invitation_id": 1234,  "invitation_secret": "abcdefghijklmno"}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.biz_reject_invitation(1234, 'abcdefghijklmno')
>>> api.commit()
```

Reject an invitation from Todoist for Business.

### Required arguments

Argument | Description
-------- | -----------
invitation_id | The invitation id.
invitation_secret | The secret fetched from the live notification.

# URL schemes

The following URL schemes might be also useful when accessing the Todoist applications (iOS and Android) or when performing certain action with them, so they are also included here for completeness.

## Views

The following schemes are available to open a specific view:

Scheme | Description
------ | -----------
todoist:// | Opens Todoist.
todoist://today | Opens the today view.
todoist://next7days | Opens the next 7 days view.
todoist://profile | Opens the profile view.
todoist://inbox | Opens the inbox view.
todoist://teaminbox | Opens the team inbox view. If the user doesn’t have a business account (access to team inbox), it will show an alert saying that he/she doesn’t have access to the team inbox because doesn’t have a business account and will be redirected automatically to the inbox view.
todoist://notifications | Opens notifications view.

## Tasks

> Example of adding a task:

```css
todoist://addtask?content=mytask&date=tomorrow&priority=4
```

The following scheme is available to create new tasks:

Scheme | Description
------ | -----------
todoist://addtask | Opens the add task view to add a new task to Todoist.

> Here's an example of a content value:

```
Create document about URL Schemes!
```

> And how it should be supplied using Percent-encoding:

```
Create&20document%20about%20URL%20Schemes%21
```

> Here's an example of a date value:

```
Tomorrow @ 14:00
```

> And how it should be supplied using Percent-encoding:

```
Tomorrow%20@%2014:00
```

The `todoist://addtask` scheme accepts the following optional values:

Value | Description
----  | -----------
content | The content of the task, which should be a string that in `Percent-encoding` (also known as URL encoding).
date | The due date of the task, which should be a string that in `Percent-encoding` (also known as URL encoding). Look at our reference to see [which formats are supported](https://todoist.com/Help/timeInsert).
priority | The priority of the task, which should be a string with a value from `1` to `4`.

If all the values are empty, it will just open the add task view. This URL Scheme will not automatically add the task to Todoist, it will just open the add task view and fill the fields.

## Projects

The following scheme is available to show all the projects:

Scheme | Description
------ | -----------
todoist://projects | Opens the projects view (shows all projects).

> Example of opening a specific project:

```
todoist://project?id=128501470
```

The following scheme is available to open a specific project:

Scheme | Description
------ | -----------
todoist://project | Opens an specific project using the id of the project.

The `todoist://project` scheme accepts the following required value:

Value | Description
----  | -----------
id | The id of the project to view. If the id doesn’t exist, you don’t have access to the project, or the value is empty, an alert will be showed and the user will be redirected to the projects view.

## Labels

The following scheme is available to show all the labels:

Scheme | Description
------ | -----------
todoist://labels | Opens the labels view (shows all labels)

> Example of opening a specific label:

```
todoist://label?id=1234
```

The following scheme is available to open a specific label:

Scheme | Description
------ | -----------
todoist://label | Opens an specific label using the id of the label.

The `todoist://label` scheme accepts the following required value:

Value | Description
----  | -----------
id | The id of the label to view. If the id doesn’t exist, you don’t have access to the label, or the value is empty, an alert will be showed and the user will be redirected to the labels view.

## Filters

The following scheme is available to show all the filters:

Scheme | Description
------ | -----------
todoist://filters | Opens the filters view (shows all filters)

> Example of opening a specific filter:

```
todoist://filter?id=9
```

The following scheme is available to open a specific filter:

Scheme | Description
------ | -----------
todoist://filter | Opens an specific filter using the id of the filter.

The `todoist://filter` scheme accepts the following required value:

Value | Description
----  | -----------
id | The id of the filter to view. If the id doesn’t exist, you don’t have access to the filter, or the value is empty, an alert will be showed and the user will be redirected to the filters view.


## Search

> Example of searching for "Test & Today":

```
todoist://search?query=Test%20%26%20Today
```

The following scheme is available to do searching:

Scheme | Description
------ | -----------
todoist://search | Used to search in the Todoist application.

The `todoist:/search` scheme accepts the following required value:

Value | Description
----  | -----------
query | The query to search in the Todoist application, which should be a string that in `Percent-encoding` (also known as URL encoding).
