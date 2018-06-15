---
layout: page
title: Server Implementation
permalink: /server/implementation/
---

Install the package:
{% highlight bash %}
npm install rotor-server --save
{% endhighlight %}

Implement on the main process of your API:
{% highlight js %}
const RotorServer = require('rotor-server');
{% endhighlight %}

## Start server
Rotor server wakes up few processes. The first one is the `turbine` process, which preloads JSON databases. The rest are clusters (except master) which process all device requests updating databases (with `turbine` process) and replicating changes (with Redis).

Don't worry about start or stop them. They will alive with your API process.
Start it by calling `start()` method:
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

`start()` method will launch Redis server (if it's dead), Rotor server and Turbine (database manager). For more information about Turbine, check out [this link](/server/turbine/).
 
`databases` param defines a list of database names for load. If any database doesn't exist, it will be created.

