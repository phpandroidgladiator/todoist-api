# Access Token


In order to make authorized calls to Todoist APIs, your application must first obtain an access token from the users. 
The sections below describe different ways of obtain such a token. 

Note that we encourage your application to use __OAuth__ protocol to obtain access token from the user, as the other authorization methods (`login` and `login_with_google` are scheduled for deprecation).



## OAuth

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
$ curl https://todoist.com/API/login_plain \
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
>>> api.login_plain('me@example.com', 'secret')
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
$ curl https://todoist.com/API/v6/login_oauth \
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
>>> api.login_oauth('me@example.com', '01234567-89ab-cdef-0123-456789abcdef')
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


## Token from Setting

You can also obtain the personal access token directly from Todoist Setting. 

Settings -> Todoist Settings -> Account -> API token
