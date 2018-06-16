---
layout: page
title: Get Started
permalink: /get-started/
---

For start working with Rotor, you must connect your server (API) and clients (only Android yet) with Redis.
> Install [Redis](../server/redis) before follow this guide.

### Prepare Rotor Server
Let's prepare a Rotor server over your API.
 
Install the package:
{% highlight bash %}
npm install rotor-server --save
{% endhighlight %}
 
And start the server:
{% highlight js %}
const RotorServer = require('rotor-server');
const RS = new RotorServer({
   "server_port": 1508,
   "turbine_port": 1510,
   "redis_port": 6379,
   "databases": ["database"],
   "debug": true
});
RS.start();
{% endhighlight %}

`start()` method will launch Redis server (if it's dead), Rotor server and Turbine (database manager). For more information about Turbine, check out [this link](https://github.com/efraespada/turbine/wiki).
 
`databases` param defines a list of database names for loading it. If any database doesn't exist, it will be created. 
### Prepare Rotor Client
Implement Rotor Core on **build.gradle** file.

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
}
{% endhighlight %}
 
> `transitive` flag is needed for implementing Rotor Core dependencies
 
Add Rotor service in `AndroidManifest.xml`:
{% highlight xml %}
<service
    android:name="com.rotor.core.RotorService"
    android:exported="false" />
{% endhighlight %}

Initialize Rotor Core in your launcher activity. Once the library connects with Redis server, application workflow can continue. 
{% highlight java %}
Rotor.initialize(getApplicationContext(), "http://10.0.2.2:1508/",
    "redis://10.0.2.2", new StatusListener() {
        @Override
        public void connected() {
            // continue to other activity or checking permissions.. 
        }
        
        @Override
        public void reconnecting() {
             
        }
    }
);
{% endhighlight %}

Now your app is ready to implement [real-time databases](/database) and [notifications](/notifications).