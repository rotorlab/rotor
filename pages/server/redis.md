---
layout: page
title: Redis
permalink: /server/redis/
---

Redis needed for sending messages to clients. Install and launch it before starting Rotor:
### Windows
Download and install latest [MSI installer](https://github.com/MicrosoftArchive/redis/releases).
Run executable.

### OSX
{% highlight bash %}
brew install redis
redis-server
{% endhighlight %}

### Ubuntu
{% highlight bash %}
sudo apt-get install redis-server
sudo service redis-server status
{% endhighlight %}
Test it on terminal:
{% highlight bash %}
// for testing with physical devices
redis-server --protected-mode no
 
// logs
redis-cli monitor
 
// test channels (sub/pub)
redis-cli PUBLISH d7bec76dac4e holi // redis-cli PUBLISH <Flamebase.id> message
{% endhighlight %}

