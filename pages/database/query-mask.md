---
layout: page
title: Queries And Masks
permalink: /database/queries-masks/
---

In the last section we have seen how to work with remote objects, but: What about doing queries to database?

Sometimes our app's workflow will need to check some data, for example:

> Database: give me all chat rooms where logged user appears

And then print all this chats in a list.

Let's see how simple is querying data.

Imagine we have a database called `myDatabase` where we store chat rooms.

{% highlight json %}
{
    "chats": {
        "123456789ASDF": {
            "id": "123456789ASDF",
            "members": {
                "EdfRtgyhBvfDc": {
                    "id": "EdfRtgyhBvfDc"
                    "rol": "admin"
                }
            }
            "messages": {
                "0": {
                    "message": "Holi 🤭"
                    "author": "EdfRtgyhBvfDc"
                }
            }
        }
    }
}
{% endhighlight %}

 
{% highlight java %}
Database.query("myDatabase",
       "/chats/*",
       "{\"members\": { \"" + 
            user.getId() + "\": { \"id\": \"" + user.getId() +
       "\" } } }",
       "{ \"id\": \"\" }",
        new QueryCallback() {
    
            @Override
            public void response(List<LinkedTreeMap<String, String>> list) {
                for(LinkedTreeMap m : list) {
                    String id = (String) m.get("id");
                    Log.d("ROTOR", "chat found: " + id);
                    // TODO load chat on list
                }
            }

        }
    );
);
{% endhighlight %}

What are we seeing here?

First parameter is the **database name** where we will look for data: `myDatabase`

Second parameter is the **path** where data should exists: `/chats/*`

Third parameter is the **query**:
{% highlight json %}
{
    "members": {
        "EdfRtgyhBvfDc": {
            "id": "EdfRtgyhBvfDc"
        }
    }
}
{% endhighlight %}

Fourth parameter is the **mask**, which will refactor the response:
{% highlight json %}
{
    "id": ""
}
{% endhighlight %}

**Query** method will always return a list of results. In this case (with a mask) the response will be:
{% highlight json %}
[
    {
        "id": "123456789ASDF"
    }
]
{% endhighlight %}

If we don't define a mask, the result will be:
{% highlight json %}
[
    {
        "id": "123456789ASDF",
        "members": {
            "EdfRtgyhBvfDc": {
                "id": "EdfRtgyhBvfDc"
                "rol": "admin"
            }
        }
        "messages": {
            "0": {
                "message": "Holi 🤭"
                "author": "EdfRtgyhBvfDc"
            }
        }
    }
]
{% endhighlight %}




