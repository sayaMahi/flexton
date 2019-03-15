---
layout: post
title: Hiding Api Keys in Android App.
date: 2018-07-02 22:00:20 +0530
description:  # Hiding Api Keys in Android App.
image:  api-key_banner.jpg # Add image post (optional)
tags: [Android Tutorials, API-KEY]
---

To avoid not having your API-KEY in your code, one of the ways is to
have API-KEY in gradle.properties of your home directory. 

Create gradle.properties file, if you dont find one. This Key resides in your local machine.

### gradle.properties (Global Properties)


![API-KEY-Location-1]({{site.baseurl}}/images/api-key-location.jpg)

{% highlight XML %}
<API-Key-Name>="<Your-API-KEY-Here>"
{% endhighlight %}

### build.gradle(Module: app)


{% highlight XML %}
apply plugin: 'com.android.application'

def SERVERNAMEDB_API_KEY = <API-Key-Name> ?: "Enter API-KEY in gradle.properties"

android {
    compileSdkVersion xx
    ...

    defaultConfig {
        ...

       each { type ->
          type.buildConfigField 'String', "ApiKey", SERVERNAMEDB_API_KEY

          type.resValue 'string', "api_key", SERVERNAMEDB_API_KEY
       }
    }

    buildTypes {
       ...
    }
}
{% endhighlight %}


"buildConfigField" field will configure the variable to be accessable.

With the above changes, 


If you need to access api-key in Java use *BuildConfig*

`Example: String api_key = BuildConfig.ApiKey`

If you need to access api-key in any Resources use *@string/api_key*

`Example: = "@string/api_key"`

Below are the few References which discuss various ways:

- <a href="http://guides.codepath.com/android/Storing-Secret-Keys-in-Android" target="_blank">Post 1</a>
- <a href="http://www.techjini.com/blog/securing-api-key-and-secret-key-in-android/" target="_blank">Post 2</a>
- <a href="http://www.androidauthority.com/how-to-hide-your-api-key-in-android-600583/" target="_blank">Post 3</a>