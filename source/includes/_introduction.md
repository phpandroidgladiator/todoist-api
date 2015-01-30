# Introduction

Welcome to the official Todoist API documentation!  The Todoist API can be used to hook [Todoist](https://todoist.com) with other applications, as it makes it easy for your client to retrieve and sync your data.

The Todoist API is based on [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer), so you can use it with a tool like [cURL](http://curl.haxx.se), but language bindings also exist for [Python](https://www.python.org), so it is easier and faster to write your own scripts or applications interacting with Todoist.

The [source code](https://github.com/Doist/todoist-api) of this API documentation is also available.

## Libraries / Tools

### Python

> You can install the Todoist Python library from PyPI with:

```
$ pip install todoist-python
```

At the moment there are language bindings for Python, in the form of the [Todoist Python API library](https://github.com/Doist/todoist-python).

A [PyPI package](https://pypi.python.org/pypi/todoist-python) has been also prepared in order to easily install the Todoist Python library in your system.

There is more detailed documentation speficically for the Todoist Python API library, and this [API reference](http://todoist-python.readthedocs.org/en/latest/) documentation can be also read online.



### JavaScript Embed

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

In the web browser environemnt, you can't use AJAX to directly communicate with Todoist due to cross domain security policy of browsers. You can solve this by communicating with Todoist using a script tag.

