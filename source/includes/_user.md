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

## Authorization

> The current API token of a user can be seen through the Todoist web application at:

```
Settings -> Todoist Settings -> Account -> API token
```

Each API call requires a valid API token, and there are two ways of obtaining an API token: via OAuth protocol or plain login authorization (which requires the user's email and password).  Using the OAuth2 method is preferred, as the plain login authorization is scheduled for deprecation, and this method will be described thereafter in detail.

External applications could obtain a user authorized API token via the OAuth2 protocol.  Developers need to register their applications before getting started.  A registered Todoist application is assigned a unique Client ID and Client Secret which are needed for the OAuth2 flow.

This procedure is comprised of 3 steps, which will be described below.

### Step 1: The authorization request

> An example of redirecting a user to the authorization URL:

```
$ curl https://todoist.com/oauth/authorize \
    -d client_id=0123456789abcdef \
    -d redirect_uri=https://example.com \
    -d scope=data:read,data:delete \
    -d state=secretstring
```

Redirect users to the authorization URL at the endpoint `https://todoist.com/oauth/authorize`, with the specified request parameters.

Here follow the required parameters:

Name | Description
---- | -----------
client_id | The unique Client ID of the Todoist application that was registered.
scope | A comma separated list of permissions that the users will grant to the application. See below a table with more details about this.
state | A unique and unguessable string. It is used for protection against cross-site request forgery attacks.

There is also an optional parameter:

Name | Description
---- | -----------
redirect_uri | The URL where users will be redirected to, after the authorization.  If left out, the redirect URI configured in the application settings will be used by default.

Here are the scope parameters mentioned before:

Name | Description
---- | -----------
task:add | Grants permission to add tasks to the Inbox project (The application cannot read tasks data).  
data:read | Grants read-only access to application data, including tasks, projects, labels, and filters. 
data:read_write | Grants read and write access to application data, including tasks, projects, labels, and filters. This scope include `task:add` and `data:read` scopes.
data:delete | Grants permission to delete application data, including tasks, labels, and filters. 
project:delete | Grants permission to delete projects.

And here are some common errors that may be encountered:

Error | Description
----- | -----------
User Rejected Authorization Request | When the user denies the authorization request, Todoist will redirect the user to the configured redirect URI with `error` paramete: `http://example.com?error=access_denied`.
Redirect URI Mismatch | The `redirect_uri` parameter must either match or be the subpath of the redirect URI which was configured in application settings. If the requirement is not satisfied, Todoist will redirect the user to the configured redirect URI with `error` parameter: `http://example.com?error=redirect_uri_mismatch`
Invalid Application Status | When an application exceeds the maximum token limit or when an application is being suspended due to abuse, Todoist will redirect the user to the configured redirect URI with the `error` parameter: `http://example.com?error=invalid_application_status`.
Invalid Scope | When the `scope` parameter is invalid, Todoist will redirect the user to the configured redirect URI with `error` parameter: `http://example.com?error=invalid_scope`.

### Step 2: The redirection to the application site

When the authorization request is granted, the user will be redirected to the `redirect_uri` specified earlier. The redirect request will come with two query parameters attached: `code` and `state`. 

The `code` parameter contains the authorization code that will be used to exchange for an access token. The `state` parameter should match the `state` parameter that was supplied in the previous step.  If the `state` is unmatched, the request has been compromised by other parties, and the process should be aborted.

### Step 3: The token exchange

> An example of exchanging the token:

```
$ curl https://todoist.com/oauth/access_token -X POST \
    -d client_id=0123456789abcdef \
    -d client_secret=secret \
    -d code=abcdef \
    -d redirect_uri=https://example.com
```

> On success, Todoist returns HTTP 200 with token in JSON object format:

```
{
  "access_token": "0123456789abcdef0123456789abcdef01234567", 
  "token_type": "Bearer"
}
```

Once the authorization `code` has been acquired, it can be exchanged it for the access token at the endpoint `POST https://todoist.com/oauth/access_token`.

Here follow the required parameters:

Name | Description
---- | -----------
client_id | The unique Client ID of the Todoist application that was registered.
client_secret | The unique Client Secret of the Todoist application that was registered.
code | The unique string code which was obtained in the previous step.

There is also an optional parameter:

Name | Description
---- | -----------
redirect_uri | The URL that was specified in the first step.  Their values must be identical.

And here are some common errors that may be encountered (all the error response will be in JSON format):

Error | Description
----- | -----------
Bad Authorization Code Error | Occurs when the `code` parameter does not match the code that is given in the redirect request: `{"error": "bad_authorization_code"}`
Incorrect Client Credentials Error | Occurs when the `client_id` or `client_secret` parameters are incorrect: `{"error": "incorrect_application_credentials"}`
Redirect URI Mismatch Error | Occurs when the `redirect_uri` does not match the redirect URL which was specified in the previous authorization request: `{"error": "redirect_uri_mismatch"}`

## Login with password

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

## Login with Google

> On success, an HTTP 200 OK with a JSON object with user data is returned:

```shell
$ curl https://todoist.com/API/v6/login_with_google \
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

Login user into Todoist with Google account.

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

## Register user

> And example of registering a new user:


```shell
$ curl https://todoist.com/API/v6/register \
    -d email=me@example.com \
    -d full_name=Example\ User \
    -d password=secret
{
  "start_day": 7,
  "start_page": "overdue, 7 days",
  "date_format": 1,
  "last_used_ip": "10.20.30.40",
  "api_token": "0123456789abcdef0123456789abcdef01234567",
  "karma_trend": "-",
  "inbox_project": 128501411,
  "time_format": 1,
  "image_id": null,
  "beta": 0,
  "sort_order": 0,
  "business_account_id": null,
  "full_name": "Example User",
  "mobile_number": null,
  "shard_id": 2,
  "timezone": "UTC",
  "is_premium": false,
  "mobile_host": null,
  "id": 1855589,
  "has_push_reminders": false,
  "is_dummy": 0,
  "premium_until": null,
  "team_inbox": null,
  "next_week": 1,
  "token": "0123456789abcdef0123456789abcdef01234567",
  "tz_offset": ["+00:00", 0, 0, 0],
  "join_date": "Wed 30 Apr 2014 13:24:38 +0000",
  "seq_no": 2180270834,
  "karma": 0.0,
  "is_biz_admin": false,
  "default_reminder": null,
  "email": "me@example.com"}
}
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI()
>>> api.register('me@example.com', 'Example User', 'secret')
{
  'start_page': 'overdue, 7 days',
  'join_date': 'Wed 30 Apr 2014 13:24:38 +0000',
  'last_used_ip': '10.20.30.40',
  'is_premium': False,
  'sort_order': 0,
  'full_name': 'Example User',
  'api_token': '0123456789abcdef0123456789abcdef01234567',
  'shard_id': 2,
  'has_push_reminders': False,
  'id': 1855589,
  'team_inbox': None,
  'next_week': 1,
  'tz_offset': ['+00:00', 0, 0, 0],
  'timezone': 'UTC',
  'email': 'me@example.com',
  'start_day': 7,
  'is_dummy': 0,
  'inbox_project': 128501411,
  'time_format': 1,
  'image_id': None,
  'beta': 0,
  'premium_until': None,
  'business_account_id': None,
  'mobile_number': None,
  'mobile_host': None,
  'date_format': 1,
  'karma_trend': '-',
  'token': '0123456789abcdef0123456789abcdef01234567',
  'seq_no': 2180270834,
  'karma': 0.0,
  'is_biz_admin': False,
  'default_reminder': None
}

```

Register a new user.

### Required parameters

Parameter | Description
--------- | -----------
email | User's email.
full_name | User's full name.
password | User's password, should be at least 5 characters long.

### Optional parameters

Parameter | Description
--------- | -----------
lang | User's language. Can be `de`, `fr`, `ja`, `pl`, `pt_BR`, `zh_CN`, `es`, `hi`, `ko`, `pt`, `ru`, `zh_TW`.
timezone | User's timezone. As default we use the user's IP address to determine the timezone.

## Delete user

> An example of deleting an existing user:

```shell
$ curl https://todoist.com/API/v6/delete_user \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d current_password=secret
"ok"
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.delete_user('secret')
ok
```

Delete an existing user.

### Required parameters

Parameter | Description
--------- | -----------
token | User's token.
current_password | User's current password.

### Optional parameters

Parameter | Description
--------- | -----------
reason_for_delete | Reason for deletion (used for feedback).
in_background | Default is `1`. Set it to `0` if you want the user deleted instantly (and not in a background worker).

## Update properties


> An example of updating the user's properties:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d api_token=0123456789abcdef0123456789abcdef01234567 \
    -d items_to_sync='[{"type": "user_update", "timestamp": "1411653994.1", "args": {"time_format": 0}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.user_update(time_format=0)
```

Update the details of the user.

### Optional parameters

Parameter | Description
--------- | -----------
email | User's email.
full_name | User's full name.
password | User's password, which should be at least 5 characters long.
timezone | User's timezone.
date_format | How should dates be formatted? If `0` `DD-MM-YYYY` will be used, if `1` `MM-DD-YYYY` will be used.
time_format | How should times be formatted? If `0` `13:00` will be used, if `1` `1:00pm` will be used.
start_day | First day of week. `1` for Monday, `7` for Sunday.
next_week | When postponing what day should we choose? `1` for Monday, `7` for Sunday.
start_page | Can be one of following values: `_blank` to show blank page, `_info_page` to show info page `_project_<PROJECT_ID>` where `<PROJECT_ID>` is the id of the project to show, `<ANY_QUERY>` to query after anything (for example `tod,tom,!!1`).
default_reminder | Can be one of the following values: `email` to send reminders by email, `mobile` to send reminders to mobile devices via SMS, `push` to send reminders to smart devices using push notifications (one of Android or iOS official client must be installed on the client side to receive these notifications), `no_default` to turn off sending default reminders.

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
