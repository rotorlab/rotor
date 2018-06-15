---
layout: page
title: Notifications Implementation
permalink: /notifications/implementation/
---

{% highlight groovy %}
android {
    defaultConfig {
        multiDexEnabled true
    }
}
 
def rotor_version =  "0.3"
 
dependencies {
    implementation ("com.rotor:core:$rotor_version@aar") {
        transitive = true
    }
    implementation ("com.rotor:notifications:$rotor_version@aar") {
        transitive = true
    }
}
{% endhighlight %}

> `transitive` flag is needed for implementing Rotor Notifications dependencies

Initialize Rotor Notifications in your launcher activity. Once the library connects with Redis server, application workflow can continue. 
{% highlight java %}
Rotor.initialize(getApplicationContext(), "http://10.0.2.2:1508/",
    "redis://10.0.2.2", new StatusListener() {
        @Override
        public void connected() {
            // uncomment if needed: Database.initialize();
            Notifications.initialize(NotificationActivity.class, 
                new Listener() {
                    @Override
                    public void opened(@NotNull String deviceId,
                                       @NotNull Notification notification) {
                        // deviceId opened notification sent
                    }
    
                    @Override
                    public void removed(@NotNull Notification notification) {
    
                    }
                }
            );
        }
        
        @Override
        public void reconnecting() {
             
        }
    }
);
{% endhighlight %}