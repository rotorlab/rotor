---
layout: page
title: Notifications
permalink: /notifications/
---

Adding a notification system never was easy. That means to send notifications,  control when to close a notification and route them.

Rotor Notifications has been designed to make it much more efficiently, even get interaction feedback from notification receivers.

The most challenging part is to know destination identifiers to send messages, and it is quite simple with Rotor Database.

#### Workflow

All notifications held on Rotor server while receivers do not interact with the message. Concretely on notifications database. Take a look:
{% highlight json %}
{
    "notifications": {
        "1528053419153": {
            "content": {
                "body": "Hey! 😊",
                "channel": "myChannel",
                "channelDescription": "Test channel",
                "id": "1528053419153",
                "photoSmall": "https://lh6.google...jc5oXQ/s96-c/photo.jpg",
                "room": "yy",
                "title": "New message",
                "requestCode": 4532
            },
            "id": "1528053419153",
            "receivers": {
                "a330a5036501e8c0": {
                    "id": "a330a5036501e8c0",
                    "viewed": 1528054161194
                }
                "b345345663456sdf3": {
                    "id": "b345345663456sdf3",
                }
            },
            "sender": {
                "id": "1c8c6b1e5a7c8300",
                "moment": 1528053419153
            },
            "time": 1528053419153
        }
    }
}
{% endhighlight %}
Once all receivers open the notification, the notification removed from the database.

#### Content
{% highlight json %}
"content": {
    "body": "Hey! 😊",
    "id": "1528053419153",
    "photoSmall": "https://lh6.google...jc5oXQ/s96-c/photo.jpg",
    "room": "yy",
    "title": "New message",
    "requestCode": 4532
}
{% endhighlight %}

The `body`, `photoSmall`, and `title` fields displayed directly on the screen.
 
The `requestCode`, `id`, and `room` fields used for routing notification interactions.
```java
private int ACTION_CHAT = 4532;
 
@Override
public void notificationTouched(int requestCode,
                                @NonNull String id,
                                @NonNull String room) {
                        
    if (action == ACTION_CHAT) {
        Intent intent = new Intent(this, ChatActivity.class);
        intent.putExtra("path", room);
        intent.putExtra("notification", id);
        startActivity(intent);
    }
    finish();
}
```
