# Live notifications

> Examples of live notifications:

```json
{
    "created": 1377639720,
    "from_uid": 123,
    "invitation_id": 456,
    "invitation_secret": "abcdefghijklmno",
    "notification_key": "notification_123",
    "notification_type": "share_invitation_sent",
    "seq_no": 12345567890,
    "state": "accepted"
}

{
    "created": 1377639720,
    "from_uid": 123,
    "invitation_id": 456,
    "notification_key": "notification_123",
    "notification_type": "share_invitation_accepted",
    "project_id": 789,
    "seq_no": 1234567890
}

{
    "created": 1377639720,
    "from_uid": 123,
    "invitation_id": 456,
    "notification_key": "notification_123",
    "notification_type": "share_invitation_rejected",
    "project_id": 789,
    "reject_email": "me@example.com",
    "seq_no": 1234567890
}

{
    "created": 1377639720,
    "from_uid": 123,
    "notification_key": "notification_123",
    "notification_type": "user_left_project",
    "project_id": 456,
    "seq_no": 1234567890
}

{
    "created": 1377639720,
    "from_uid": 123,
    "notification_key": "notification_123",
    "notification_type": "user_removed_from_project",
    "project_id": 456,
    "removed_name": "Example User",
    "removed_uid": 789,
    "seq_no": 1234567890
}

{
    "assigned_by_uid": 789,
    "created": 1377639720,
    "from_uid": 123,
    "item_content": "NewTask",
    "item_id": 456,
    "notification_key": "notification_123",
    "notification_type": "item_assigned",
    "project_id": 789,
    "responsible_uid": 321,
    "seq_no": 1234567890
}

{
    "assigned_by_uid": 789,
    "created": 1377639720,
    "from_uid": 123,
    "item_content": "NewTask",
    "item_id": 456,
    "notification_key": "notification_123",
    "notification_type": "item_completed",
    "project_id": 789,
    "responsible_uid": 321,
    "seq_no": 1234567890
}

{
    "assigned_by_uid": 789,
    "created": 1377639720,
    "from_uid": 123,
    "item": 456,
    "item_content": "NewTask",
    "notification_key": "notification_123",
    "notification_type": "item_uncompleted",
    "project": 789,
    "responsible_uid": 321,
    "seq_no": 1234567890
}

{
    "created": 1377639720,
    "from_uid": 123,
    "item_id": 456,
    "note_content": "NewTask",
    "note_id": 789,
    "notification_key": "notification_123",
    "notification_type": "note_added",
    "project_id": 321,
    "seq_no": 1234567890
}

{
    "created": 1377639720,
    "email": "me@example.com",
    "from_uid": 123,
    "notification_key": "notification_123",
    "notification_type": "biz_policy_disallowed_invitation",
    "project_id": 456,
    "seq_no": 1234567890,
    "user": {
        "email": "you@example.com",
        "full_name": "Example User",
        "id": "789",
        "image_id": "321"
    }
}

{
    "created": 1377639720,
    "from_uid": 123,
    "inviter_id": 456,
    "notification_key": "notification_123",
    "notification_type": "biz_policy_rejected_invitation",
    "seq_no": 1234567890,
    "user": {
        "email": "you@example.com",
        "full_name": "Example User",
        "id": "789",
        "image_id": "321"
    }
}

{
    "active_until": 1399299727,
    "created": 1377639720,
    "from_uid": 123,
    "notification_key": "notification_123",
    "notification_type": "biz_trial_will_end",
    "plan": "business_monthly",
    "quantity": 10,
    "seq_no": 1234567890
}

{
    "active_until": 1399299727,
    "amount_due": 600,
    "attempt_count": 1,
    "created": 1377639720,
    "currency": "usd",
    "description": "2 x Subscription to Monthly ($3.00/month)",
    "from_uid": 123,
    "next_payment_attempt": 1399299735,
    "notification_key": "notification_123",
    "notification_type": "biz_payment_failed",
    "plan": "business_monthly",
    "quantity": 10,
    "seq_no": 1234567890
}

{
    "active_until": 1399299727,
    "created": 1377639720,
    "from_uid": 123,
    "notification_key": "notification_123",
    "notification_type": "biz_account_disabled",
    "plan": "business_monthly",
    "quantity": 10,
    "seq_no": 1234567890
}

{
    "account_name": "Example Inc.",
    "created": 1377639720,
    "from_uid": 123,
    "from_user": {
        "email": "you@example.com",
        "full_name": "Example User",
        "id": "456",
        "image_id": "789"
    },
    "invitation_id": 321,
    "invitation_message": "Welcome to our team!",
    "invitation_secret": "abcdefghijklmno",
    "notification_key": "notification_123",
    "notification_type": "biz_invitation_created",
    "seq_no": 1234567890
}

{
    "created": 1377639720,
    "from_uid": 123,
    "from_user": {
        "account_name": "Example Inc.",
        "email": "you@example.com",
        "full_name": "Example User",
        "id": "456",
        "image_id": "789"
    },
    "invitation_id": 321,
    "notification_key": "notification_123",
    "notification_type": "biz_invitation_accepted",
    "seq_no": 1234567890
}

{
    "created": 1377639720,
    "from_uid": 123,
    "from_user": {
        "account_name": "Example Inc.",
        "email": "you@example.com",
        "full_name": "Example User",
        "id": "456",
        "image_id": "789"
    },
    "invitation_id": 321,
    "notification_key": "notification_123",
    "notification_type": "biz_invitation_rejected",
    "seq_no": 1234567890
}
```

### Types

This is the list of notifications which can be issued by the system:

Type | Description
-----|------------
share_invitation_sent | Sent to the sharing invitation receiver.
share_invitation_accepted | Sent to the sharing invitation sender, when the receiver accepts the invitation.
share_invitation_rejected | Sent to the sharing invitation sender, when the receiver rejects the invitation.
user_left_project | Sent to everyone when somebody leaves the project.
user_removed_from_project | Sent to everyone, when a person removes somebody from the project.
item_assigned | Sent to user who is responsible for the task. Optionally it's also sent to the user who created the task initially, if the assigner and the task creator is not the same person.
item_completed | Sent to the user who assigned the task when the task is completed. Optionally it's also sent to the user who is responsible for this task, if the responsible and the user who completed the task is not the same person.
item_uncompleted | Sent to the user who assigned the task when the task is uncompleted. Optionally it's also sent to the user who is responsible for this task, if the responsible and the user who completed the task is not the same person.
note_added | Sent to all members of the shared project, whenever someone adds a note to the task.
biz_policy_disallowed_invitation | Sent to you when you try to share a project with someone outside of your business account, but the business account policy disallows this action. 
biz_policy_rejected_invitation | Sent to you when you try to accept the invitation to a shared project from someone outside of your business account, but the business account policy disallows this action. 
biz_trial_will_end | Sent to all business account administrators three days before the trial period of a subscription is scheduled to end.
biz_payment_failed | Sent to all business account administrators whenever an invoice attempts to be paid, and the payment fails. This can occur either due to a declined payment, or because the customer has no active card. A particular case of note is that if a customer with no active card reaches the end of its free trial.
biz_account_disabled | Sent to all business account administrators when the account is disabled.
biz_invitation_created | Sent to an invitee, when one of business account administrators invites this user to the business account.
biz_invitation_accepted | Sent to an inviter, when the invitation is accepted.
biz_invitation_rejected | Sent to an inviter, when the invitation is rejected.

### Properties

Some properties are common for all types of notifications, whereas some others depend on the notification type.

Every live notification has the following properties: 

Property | Description
-------- | -----------
created | Required. Live notification creation date. Integer representing a timestamps since epoch.
from_uid | Required. The id of user who initiated this live notification.
notification_key | Required. Unique notification key.
notification_type | Required. Type of notification. Different notification type define different extra fields which are described below.
seq_no | Required. Notification sequence number.
from_user | Optional. User data, useful on `share_invitation_sent`.
project_name | Optional. The project name, useful for `share_invitation_*` where you may not have the project in the local model.
invitation_secret | Optional. Useful for accepting/rejecting invitations.

Here are the extra properties for the `share_invitation_sent` type of live notifications:

Property | Description
-------- | -----------
state | Invitation state. Initially `invited`, can change the state to `accepted` or `rejected`.

Here are the extra properties for the `user_removed_from_project` type of live notifications:

Property | Description
-------- | -----------
removed_name | The name of the user removed.
removed_uid | The uid of the user removed.

Here are the extra properties for the `biz_trial_will_end` type of live notifications:

Property | Description
-------- | -----------
quantity | The number of users under the control of the business account.
plan | Tariff plan name. Valid values are `business_monthly` and `business_yearly`.
active_until | The timestamp when the business account will be disabled. The value may not match the business account subscription end date, as we give some extra days (up to two weeks) to pay the invoice.

Here are the extra properties for the `biz_payment_failed` type of live notifications:

Property | Description
-------- | -----------
quantity | The number of users under the control of the business account.
plan | Tariff plan name. Valid values are `business_monthly` and `business_yearly`.
active_until | The timestamp when the business account will be disabled. The value may not match the business account subscription end date, as we give some extra days (up to two weeks) to pay the invoice.
amount_due | Invoice amount. Integer value in `0.01` of currency.
attempt_count | Number of automatic payment attempts made for this invoice.
currency | Currency value. Three-letter ISO currency code representing the currency in which the charge was made.
description | Invoice description.
next_payment_attempt | Timestamp value.

Here are the extra properties for the `biz_account_disabled` type of live notifications:

Property | Description
-------- | -----------
quantity | The number of users under the control of the business account.
plan | Tariff plan name. Valid values are `business_monthly` and `business_yearly`.
active_until | The timestamp when the business account will be disabled. The value may not match the business account subscription end date, as we give some extra days (up to two weeks) to pay the invoice.

Here are the extra properties for the `biz_invitation_created` type of live notifications:

Property | Description
-------- | -----------
state | Invitation state. Initially `invited`, can change the state to `accepted` or `rejected`.
invitation_secret | Invitation secret. Should be used to accept or reject invitation.
invitation_message | Invitation message.
account_name | Business account (company) name.

## Mark last read

> An example of marking the last read notification:

```shell
$ curl https://todoist.com/API/v6/sync -X POST \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "live_notifications_mark_as_read", "uuid": "1412581522.1", "args": {"seq_no": 1234}}]'
{ ...
  "SyncStatus": [{"status": "ok", "uuid": "1412581522.1"}],
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.live_notifications.mark_as_read(1234)
>>> api.commit()
```

Mark the last read live notification.

### Required arguments

Argument | Description
-------- | -----------
seq_no | The sequence number of the last read notification.
