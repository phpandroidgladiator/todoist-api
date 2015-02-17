# Introduction

Welcome to the official Todoist API documentation!  The Todoist API can be used to hook [Todoist](https://todoist.com) with other applications, as it makes it easy for your client to retrieve and sync your data.

The Todoist API is based on [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer), so you can use it with a tool like [cURL](http://curl.haxx.se), but language bindings also exist for [Python](https://www.python.org), so it is easier and faster to write your own scripts or applications interacting with Todoist.

The [source code](https://github.com/Doist/todoist-api) of this API documentation is also available.

## Libraries

### Python

> You can install the Todoist Python library from PyPI with:

```
$ pip install todoist-python
```

At the moment there are language bindings for Python, in the form of the [Todoist Python API library](https://github.com/Doist/todoist-python).

A [PyPI package](https://pypi.python.org/pypi/todoist-python) has been also prepared in order to easily install the Todoist Python library in your system.

There is more detailed documentation speficically for the Todoist Python API library, and this [API reference](http://todoist-python.readthedocs.org/en/latest/) documentation can be also read online.

## JSON-P Callbacks

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

The Todoist API supports [JSONP](http://en.wikipedia.org/wiki/JSONP) which allows you to get around the cross-domain policy and to request data from Todoist in the browser environment.


## Overview

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
login | Logins to the server using plain email/password.
login_with_google | Logins to the server OAuth2.
register | Registers a new account.
delete_user | Deletes an existing account.
get_redirect_link | Gets the redirect link.
get_productivity_stats | Gets the productivity stats.
update_notification_settings | Updates the notification settings.
get_all_completed_tasks | Gets the user's completed tasks.
