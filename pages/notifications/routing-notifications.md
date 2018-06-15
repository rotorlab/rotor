---
layout: page
title: Activity Routing
permalink: /notifications/routing/
---

For routing notification interactions you need a type of `SplashActivity`. This type is needed because the app's screen should be ready at the moment we open the app (explained above).
> A SplashActivity shouldn't have any layout, only a theme defined for a faster load

Define a routing activity by extending `NotificationRouterActivity` and implement `onCreate` and `notificationTouched` methods:
{% highlight java %}
public class NotificationActivity extends NotificationRouterActivity {

    private static String TAG = NotificationActivity.class.getSimpleName();
    private int ACTION_CHAT = 4532;

    @Override
    public void onCreate() {
        Rotor.initialize(getApplicationContext(), "http://10.0.2.2:1508/",
                         "redis://10.0.2.2", new StatusListener() {
            @Override
            public void connected() {
                Database.initialize();
                Notifications.initialize(...);
            }

            @Override
            public void reconnecting() {

            }
        });
    }

    @Override
    public void notificationTouched(int action,
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

}
{% endhighlight %}
Note there is no "savedInstanceState" param on "onCreate" method. That's because we don't need to control anything on this activity, only route the interaction with a notification.

You can stylish it with a theme:
{% highlight xml %}
<activity
    android:name=".NotificationActivity"
    android:screenOrientation="portrait"
    android:theme="@style/SplashTheme"/>
{% endhighlight %}
Define the theme style:
{% highlight xml %}
<resources>

    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
    </style>

    <style name="SplashTheme" parent="AppTheme">
        <item name="android:windowBackground">@drawable/splash</item>
    </style>

</resources>
{% endhighlight %}
And a drawable. Here is used the application's icon:
{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android"
    android:opacity="opaque">
    <item android:drawable="@color/icons"/>
    <item>
        <bitmap
            android:gravity="center"
            android:src="@mipmap/ic_launcher_rounded"/>
    </item>
</layer-list>
{% endhighlight %}

