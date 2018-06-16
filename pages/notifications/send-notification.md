---
layout: page
title: Send Notification
permalink: /notifications/send/
---

Build and send notifications is very easy:
{% highlight java %}
Content content = new Content(ACTION_CHAT,  // requestCode
        "Hi :)",                            // title
        "Welcome to notifications!",        // body
        "my_room",                          // room
        "myChannel",                        // channel ID           (Android 8)
        "Test channel",                     // channel description  (Android 8)
        "https://lh6.google...oto.jpg",     // small photo url
        "https://lh6.google...oto.jpg",     // big photo url
);                              
 
ArrayList<String> ids = new ArrayList<>();  // device ids that will receive
                                            // the notification
ids.add("56783g345guik456"); // <-- device id A
ids.add("48484aad18e02d76"); // <-- device id B

// send
Notifications.notify(Notifications.builder(content, ids));
{% endhighlight %}

Be careful sending notifications to yourself. Only should do for testing purposes.
{% highlight java %}
Content content = new Content(...);                              
 
ArrayList<String> ids = new ArrayList<>();       
ids.add(Rotor.id); // <-- don't do it in production environments

Notifications.notify(Notifications.builder(content, ids));
{% endhighlight %}
