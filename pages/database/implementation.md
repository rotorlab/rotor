---
layout: page
title: Database Implementation
permalink: /database/implementation/
---

Import libraries:

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
    implementation ("com.rotor:database:$rotor_version@aar") {
        transitive = true
    }
}
{% endhighlight %}

> `transitive` flag is needed for implementing Rotor Core dependencies
 
Initialize database module after Rotor Core initialization. Should be invoked on `LoadingActivity` or `SplashActivity`:
{% highlight java %}
Rotor.initialize(getApplicationContext(), "http://10.0.2.2:1508/",
    "redis://10.0.2.2", new StatusListener() {
        @Override
        public void connected() {
             Database.initialize()
             
             // login - main UI
             // start listen references
        }
        
        @Override
        public void reconnecting() {
             
        }
    }
);
{% endhighlight %}

