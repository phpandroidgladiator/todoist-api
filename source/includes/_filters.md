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
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "filter_add", "temp_id": "$1412333089.1", "uuid": "1412333089.1", "args": {"name": "Filter1", "query": "no due date"}}]'
{ ...
  "SyncStatus": [{"status": "ok", "uuid": "1412333089.1"}],
  "TempIdMapping": {"$1412333089.1": 9},
  ... }

```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> filter = api.filters.add('Filter1', 'no due date')
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
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "filter_update", "uuid": "1412334986.1", "args": {"id": 9, "query": "tomorrow"}}]'
{ ...
  "SyncStatus": [{"status": "ok", "uuid": "1412334986.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> filter = api.filters.get_by_id(9)
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
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "filter_delete", "uuid": "1412336184.1", "args": {"id": 9}}]'
{ ...
  "SyncStatus": [{"status": "ok", "uuid": "1412336184.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> filter = api.filters.get_by_id(9)
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
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands=[{"type": "filter_update_orders", "uuid": "1412336529.1", "args": {"id_order_mapping": {"9":  1, "10": 2}}}]'
{ ...
  "SyncStatus": [
    { "status":
      [ {"9": "ok"},
        {"10": "ok"} ],
      "uuid": "1412336529.1" },
  ],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.filters.update_orders({9: 1, 10: 2})
>>> api.commit()
```

### Required arguments

Argument | Description
-------- | -----------
id_order_mapping| A dictionary, where a filter id is the key, and the order its value: `filter_id: order`.
