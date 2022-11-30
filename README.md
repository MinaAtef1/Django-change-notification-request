# Django Change Notification Request

## Introduction 
Some changes need user approval first, and we need to allow our users to make a decision before changing the database.
This package is made just for that.

## How It Works
![ex](https://user-images.githubusercontent.com/36309814/204921480-891fd15f-8dd9-4d00-9c35-92ad76112e49.png)

I think an example is the best way to explain this.

1. A function will change an object, but instead of saving it, it will send a notification with the object.
```python
from services.notification import send_change_notification

def some_function_that_make_some_changes():
    # ....
    # very long and boring logic here
    object_that_needs_to_be_updated = TestModel("some data")
    # here comes the magic
    send_change_notification(object_that_needs_to_be_updated,title="title", description="description", priority=2)
```
This will create a notification record with the changes that the function made, ready to be saved based on the user's decision.
and a notification can have multiple changes from multiple different objects.

2. A user will receive the notification, but first the user needs to subscribe to an object, table, or a queryset
![image](https://user-images.githubusercontent.com/36309814/204923526-520b5c42-0405-41e1-987d-4651a53bebb5.png)

A user can choose a table to subscribe to all of its notifications.
or a specific object with the id
or a query set
A user must subscribe to an object before being able to accept any notification on

4. Accept or decline the notification.
```python 
from services.notification import get_user_notifications, get_object_notifications


accept_change_notification(notification, user)
decline_change_notification(notification, user)
```

a notification has the object as the target, the data that was changed
and when it gets accepted, the data is then saved to the database.

a better notification view shpuld be added to the admin to view the notification
with another view to see the changes and accept or deny them.
