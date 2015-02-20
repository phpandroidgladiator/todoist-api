# Sharing projects

Commands that are related to sharing projects will be described in this section.

## Share a project

> An example of sharing a project:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "share_project", "temp_id": "$1412585639.1", "timestamp": "1412585639.1", "args": {"project_id": "128501470", "message": "", "email": "you@example.com"}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1412585639.1"}],
  ... }
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
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "delete_collaborator", "timestamp": "1412586651.1", "args": {"project_id": 128501470, "email": "you@example.com"}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1412586651.1"}],
  ... }
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
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "accept_invitation", "timestamp": "1412587467.1", "args": {"invitation_id": 1234,  "invitation_secret": "abcdefghijklmno"}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1412587467.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.invitations.accept(1234, 'abcdefghijklmno')
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
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "reject_invitation", "timestamp": "1412587669.1", "args": {"invitation_id": 1234,  "invitation_secret": "abcdefghijklmno"}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1412587669.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.invitations.reject(1234, 'abcdefghijklmno')
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
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "delete_invitation", "timestamp": "1412587915.1", "args": {"invitation_id": 128501470}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1412587915.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.invitations.delete(128501470)
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
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "take_ownership", "timestamp": "1412588044.1", "args": {"project_id": 128501470}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1412588044.1"}],
  ... }
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
project_id | The shared project of which to take the ownership.

## Accept a business invitation

> An example of accepting a business invitation:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "biz_accept_invitation", "timestamp": "1412588256.1", "args": {"invitation_id": 1234,  "invitation_secret": "abcdefghijklmno"}}]'
{ ...
  "SyncStatus": [{"status": "ok", "timestamp": "1412588256.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.biz_invitations.accept(1234, 'abcdefghijklmno')
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
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "biz_reject_invitation", "timestamp": "1412588264.1", "args": {"invitation_id": 1234,  "invitation_secret": "abcdefghijklmno"}}]'
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.biz_invitations.reject(1234, 'abcdefghijklmno')
>>> api.commit()
```

Reject an invitation from Todoist for Business.

### Required arguments

Argument | Description
-------- | -----------
invitation_id | The invitation id.
invitation_secret | The secret fetched from the live notification.
