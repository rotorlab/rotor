---
layout: page
title: Paths And Models
permalink: /server/paths-models/
---

Objects are located in the database by paths. Let's imagine we want to generate or work with this database model:
{% highlight json %}
{
    "chats": {
        "chatA": {
            "title": "My chat",
            "members": {
                "774562384": {
                    "rol": "admin"
                },
                "775635684": {
                    "rol": "basic"
                }
            },
            "messages": {
                "djwersdveg": {
                    "message": "Hi John!",
                    "author": "775635684"
                },
                "djhsfdgssg": {
                    "message": "Hi!",
                    "author": "774562384"
                }
            }
        }
    },
    "users": {
        "774562384": {
            "name": "John",
            "birth": 1993
        },
        "775635684": {
            "name": "Mark",
            "birth": 1993
        }
    }
}
{% endhighlight %}

 
### Samples

`/chats/chatA/members`
{% highlight json %}
{
    "774562384": {
        "rol": "admin"
    },
    "775635684": {
        "rol": "basic"
    }
}
{% endhighlight %}

 
`/users/774562384`
{% highlight json %}
{
    "name": "John",
    "birth": 1993
}
{% endhighlight %}

 
### Models
It’s so easy to work with JSON objects. In JavaScript it's done as associative arrays:{% highlight js %}
let object = {}
object.name = "John"

if (object.birth === undefined) {
    object.birth = 1993
}
 
delete object["name"]

let string = JSON.stringify(object)
let result = JSON.parse(string)
 
console.log("birth: " + result.birth)
{% endhighlight %}

In Android, Rotor works with Java or Kotlin apps. For work with JSON objects in Java define classes with the fields of the desired model. Rotor libraries work with GSON and make that work for you. Check **[remote objects](../../database/remote)** page for more documentation:
{% highlight java %}
class User {
 
    SerializedName("name")
    String name;
 
    SerializedName("birth")
    int birth;
 
    public User() {
    }
 
    public User(String name, int birth) {
        this.name = name;
        this.birth = birth;
    }

    public void setName(String name) {
        this.name = name;
    }
 
    public void getName() {
        return name;
    }
 
    public void setBirth(int birth) {
        this.birth = birth;
    }
 
    public void getBirth() {
        return birth;
    }
}
{% endhighlight %}

In Kotlin you can't work with data classes for JSON de/serialization. Use Java objects instead.
> I couldn't do GSON work with data class with `var` fields (variables). With `val` fields (values) works, but it's immutable
