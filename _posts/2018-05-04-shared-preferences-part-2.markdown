---
layout: post
title: Shared Preferences - Part 2
date: 2018-05-04 11:00:20 +0530
description:  # Shared Preferences - Setup, Accessing Files (optional)
image:  prefbanner_2.jpg # Add image post (optional)
tags: [Android Tutorials, Shared Preferences]
---

This is in continuation to [Part 1][preferencebasic-1]

<form>
<input style="background:#3366cc; cursor: pointer; color: #fff; border-radius: 3px; border: 1px solid #3366cc;" class='c-btn' type="button" value="Download Code" onclick="window.open('http://github.com/sayaMahi/SharedPreferenceExample')" target="_blank" />
</form>

In this post, we shall be working on already created Default Shared Preferences File.

We can create SharedPreferences at 2 levels. one is Application level, another is Activity level.

In previous post, we have seen values being saved to SharedPreferences file automatically by the Preference Objects.

In order to read the values or write to the file programatically, we need to get the handle of the SharedPreferences file.


### To get the handle of SharedPreferences File:

SharedPreferences sharedpref = 

* `= PreferenceManager.getDefaultSharedPreferences(this);`

> This gets a SharedPreferences instance that points to the default file. 

* `= getSharedPreferences("PrefFileName", Context.MODE_PRIVATE);` 

> This gets a SPECIFIC SharedPreferences instance by NAME.

* `= getPreferences(Context.MODE_PRIVATE);`

> This gets the handle for the Activity level SharedPreferences File. File name will be same as the Activity Class name.

Having Context.MODE_PRIVATE specify will make sure the file is accessable only by the application.

### Write:  :pencil2:

{% highlight JAVA %}

//get the editor object
SharedPreferences.Editor edit = sharedpref.edit();

// Add key-value pairs 
edit.putString("username", "Mahi");
edit.putBoolean("session_active", true);
edit.putFloat(String key, float value);
edit.putInt(String key, int value)
edit.putStringSet(String key, Set<String> values);
 
// Commit or Apply the changes
edit.commit(); // work is done Synchronously on the main thread.
edit.apply();  // work is done off the main thread.

{% endhighlight %}


### Delete:  :x:

{% highlight JAVA %}
//Delete
sharedpref.remove("key_name");

// Commit or Apply the changes
edit.commit(); // work is done Synchronously on the main thread.
edit.apply();  // work is done off the main thread.
{% endhighlight %}


### Read:  :book:

Once you have the SharedPreferences object, Reading the values is direct. Based on the datatype, use the appropriate methods like getBoolean(), getString(), getFloat(), getInt(), etc. 

Read all the methods [here][SharedPreferences-3]

Example:
{% highlight JAVA %}
//Reading Boolean value
boolean value = sharedpref.getBoolean("key_name", true /*default value*/);
// Reading String value
String stringValue = sharedpref.getString("key_name", "12");
{% endhighlight %}

If the key doesn't exist, the default value (2nd argument) will be returned by getBoolean().

If the user changes the preference setting, in-order to get notified about the new value during run time, Listener has to be set.

In the example above, if the String value stored in the file is a combination of numberic, if the user enteres "1Z" by mistake, you will see undesired results based on how you use this value. To inform the user about this wrong input dynamically, Listener has to be set.

### Listeners:  :ear:

These are call back functions when there are any changes to file or changes to input fields.

There are 2 SharedPreferences Listeners:

* SharedPreferences.OnSharedPreferenceChangeListener

> is triggered, AFTER any value is changed in the SharedPreferences File.


* Preference.OnPreferenceChangeListener

> is triggered, BEFORE any value is saved to the SharedPreferences File.


**Implementing Listeners:** 

{% highlight JAVA %}
implements SharedPreferences.OnSharedPreferenceChangeListener, 
  Preference.OnPreferenceChangeListener{

 @Override
 public boolean onPreferenceChange(Preference preference, Object newValue) {

 /* ******************
 this function is called, BEFORE any value is saved to the SharedPreferences File.

 //Do necessary checks and inform user if need be.
 ****************** */

 }

@Override
 public void onSharedPreferenceChanged(SharedPreferences sharedPreferences, String key) {

 /* ******************
 this function is called, AFTER any value is changed in the SharedPreferences File.

 //Read the new values here.
 ****************** */

 }
}
{% endhighlight %}

**Register/Un-Register Listener:**

{% highlight JAVA %}
    @Override
    public void onCreate(Bundle savedInstanceState) {
        ...
        // Register the Listener
        getPreferenceScreen().getSharedPreferences()
                .registerOnSharedPreferenceChangeListener(this);
    }
    @Override
    public void onDestroy() {
        ...
        // Un-Register the Listener
        getPreferenceScreen().getSharedPreferences()
        .unregisterOnSharedPreferenceChangeListener(this);
    }
{% endhighlight %}


If this post helped or have queries please leave ur comments and share.

[preferencebasic-1]: http://www.androidcitizen.com/shared-preferences-part-1/
[SharedPreferences-3]: http://developer.android.com/reference/android/content/SharedPreferences
[SharedPreferences-Code]: http://github.com/sayaMahi/SharedPreferenceExample
