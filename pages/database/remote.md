---
layout: page
title: Working With Remote Objects
permalink: /database/remote/
---

Rotor Database allows devices to work with the same objects by listening to the same `path`. When an object has listened, the library says to Rotor server your device is waiting for changes on that `path`, so every time any device makes a change on that (object), the differences are calculated and replicated on all devices listening.
 
Rotor Database works with Java objects.
{% highlight java %}
class ObjectA {
 
    @SerializedName("value")
    @Expose
    String value;
 
    public ObjectA() {
        // nothing to do here
    }
 
    public ObjectA(String value) {
        this.value = value;
    }
 
    public void setValue(String value) {
        this.value = vaue;
    }
 
    public void getValue() {
        return value;
    }
 
}
{% endhighlight %}
 
## Listen
 
`listen(database: String, path: String, object: Reference<T>(T::class.java))` method has a simple **object lifecycle interface** which controls every object state.

{% highlight kotlin %}
// kotlin
var objectA: ObjectA ? = null
 
Database.listen("database",                       // database name
       "/myObjects/objectA",                      // object path
       Reference<ObjectA>(ObjectA::class.java) {  // reference lifecycle
 
          fun onCreate()
 
          fun onUpdate(): ObjectA ?
 
          fun onChanged(ref: ObjectA)
    
          fun onDestroy()
 
          fun progress(value: Int)
               
      }
)
{% endhighlight %}


#### onCreate
Called when object is not created in remote DB yet. Object is defined and synchronized with server here. This method won't be called if object already exists on server, `onChange` method will be called instead.

{% highlight java %}
// java
@Override
public void onCreate() {
    objectA = new ObjectA("foo");
    Database.sync(path);
}
{% endhighlight %}

#### onChanged
Called in two situations, when some device has made changes on the same object and when `listen` method is called and the object is cached. Database library pass the object up to date as parameter.
{% highlight java %}
@Override
public void onChanged(ObjectA objectA) {
    this.objectA = objectA;  
    // notify change on UI
}
{% endhighlight %}

#### onUpdate
Called when `sync` method is invoked. Differences with the last "fresh" object passed by library are calculated and sent to server.
{% highlight java %}
@Override
public ObjectA onUpdate() {
    return objectA;
}
{% endhighlight %}

#### onDestroy
Called when `remove` method is invoked. Reference is removed on database too.
{% highlight java %}
@Override
public void onDestroy() {
    objectA = null;
    // UI changes for removed object
}
{% endhighlight %}

#### progress
Some object updates can become too big, so server slices updates and sends them sequentially. value parameter goes from 0 to 100
{% highlight java %}
@Override
public void progress(int value) {
    Log.e(TAG, "loading " + path + " : " + value + " %");
}
{% endhighlight %}

## Sync

After work with objects, changes must be synchronized with servers. Call `sync(path: String)` method to sync it.
{% highlight java %}
Database.sync("/myObjects/objectA");
{% endhighlight %}

## Unlisten

Remove listener in server by calling `unlisten(path: String)`.
{% highlight java %}
Database.unlisten("/myObjects/objectA");
{% endhighlight %}

## Remove

Remove reference in database by calling `remove(path: String)`. `onDestroy` will be called.
{% highlight java %}
Database.remove("/myObjects/objectA");
{% endhighlight %}
