# Reminders

> A reminder in Todoist is a JSON object. Typically a reminder object will look like:

```shell
{
  "due_date": "Mon 06 Oct 2014 11:00:00 +0000",
  "due_date_utc": "Mon 06 Oct 2014 11:00:00 +0000",
  "is_deleted": 0,
  "service": "email",
  "item_id": 33511505,
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
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "reminder_add", "temp_id": "$1412573453.1", "timestamp": "1412573453.1", "args": {"item_id": 33511505, "service": "email", "minute_offset": 30}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1412573453.1"}],
  "TempIdMapping": {"$1412573453.1": 1234},
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> reminder = api.reminders.add(33511505, service='email', minute_offset=30)
>>> api.commit()
```

> An example of adding an absolute reminder:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "reminder_add", "temp_id": "$1412573453.1", "timestamp": "1412573453.1", "args": {"item_id": 33511505, "service": "email", "due_date_utc": "2014-10-15T11:00"}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1412573453.1"}],
  "TempIdMapping": {"$1412573453.1": 1234},
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> reminder = api.reminders.add(33511505, service='email', due_date_utc='2014-10-15T11:00')
>>> api.commit()
```

> An example of adding a location reminder:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "reminder_add", "temp_id": "$1412573453.1", "timestamp": "1412573453.1", "args": {"item_id": 33511505, "service": "email", "type": "location", "name": "Aliados", "loc_lat": "41.148581", "loc_long":"-8.610945000000015", "loc_trigger":"on_enter", "radius": 100}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1412573453.1"}],
  "TempIdMapping": {"$1412573453.1": 1234},
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> reminder = api.reminders.add(33511505, service='email', type='location', name='Aliados', loc_lat='41.148581', loc_long='-8.610945000000015', loc_trigger='on_enter', radius=100)
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
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "reminder_update", "timestamp": "1412581522.1", "args": {"id": 1234, "due_date_utc": "2014-10-10T15:00"}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1412581522.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> reminder = api.reminders.get_by_id(1234)
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
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "reminder_delete", "timestamp": "1412336184.1", "args": {"id": 9}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1412336184.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> reminder = api.reminders.get_by_id(1234)
>>> reminder.delete()
>>> api.commit()
```

Delete a reminder.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the filter.
