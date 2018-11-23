---
layout: post
title: Shared Preferences - Part 1
date: 2018-05-02 09:00:20 +0530
description:  # Shared Preferences - Setup, Accessing Files (optional)
image:  prefbanner_1.jpg # Add image post (optional)
tags: [Android Tutorials, Shared Preferences]
---

Part 1: (This post) Discussess about What is SharedPreference, How to build the Settings Screen.

[Part 2][preferencebasic-2] : Discussess about Reading/Writing data from SharedPreference File.

<form>
<input style="background:#3366cc; cursor: pointer; color: #fff; border-radius: 3px; border: 1px solid #3366cc;" class='c-btn' type="button" value="Download Code" onclick="window.open('https://github.com/sayaMahi/SharedPreferenceExample')" target="_blank" />
</form>

Shared Preferences are basically Settings that User sets either on Phone or App. So these settings should remain unchanged, even on Phone reboots, untill User changes it. These changes can be a Switch toggle, a Checkbox selection, display of Text on Lock Screen, a Ringtone selection (from a List) etc.

Basically, For one Label, there is One selection. Its Key-Value pair. 
Example: (Switch, OFF), (Checkbox, Selected).

These changes can be saved on the Device storage in a XML File which supports Key-Value pairs.

A seperate activity/dialog can be created in a traditional way to have different settings options. But, Android Framework provides the library (Preferences library) which does just that.

Any changes made on the settings, are saved to the file automatically as key-value pair.

Values can be of type - Boolean, Float, Int, Long,  String, String set. 

By default, default name of the preference file & location will be 
> Name:  `<package-name>_preferences.xml` 

> Location: `/data/data/package-name/shared_prefs` 

Device File Explorer can be found on AndroidStudio, at right most bottom (like show in below image), alternatively can be searched by pressing **Command/Ctrl + Shift + A** to open the Actions menu and search for “Device File Explorer”.

![Preference-File-Location]({{site.baseurl}}/images/pref_file_location.jpg)

If you open the File, it will look like this (example used in this post)

![Preference-File]({{site.baseurl}}/images/preferences_file.jpg)

Inorder to Populate Activity with Preferences, we need to make use of PreferenceFragmentCompat Class which is an extention of Fragment class (which will populate all the Preference Objects as Lists).

There are two types of PreferenceObjects. 
* TwoStatePreference Class. 

	> Objects based on this have 2 selectable states like Checkbox selection.

* DialogPreference Class. 

	> Objects based on this are Dialog based like List selection.

Few Preference Objects:

* EditTextPreference
* CheckboxPreference
* SwitchPreference
* ListPreference 
* etc..

For each Preference Object, there are 3 minimum things which need to be set while building the settigs screen.

1. Key.           (This will be stored in Preferences XML file)
2. Default Value. (This will be stored in Preferences XML file)
3. Title. 		
4. Summary. (Optional. This field will help the User identify the state)

The root of Preferences will be PreferenceScreen. Nested PreferenceScreen will denote Screen Breaks, and to navigate different PreferenceScreens onNavigateToScreen(PreferenceScreen) to be used.

Things to do, for building the PreferenceScreen.
* Add library
* Build preference_layout XML, to include all the Preferences.
* Build a class and inherit PreferenceFragmentCompat Class.
	*  override onCreatePreferences and load preference_layout XML.
	`addPreferencesFromResource(R.xml.preferences);`
* In the Settigs activity layout, include the fragment class which you created above.
* Add PreferenceTheme in Styles.

## We shall be building below example in this post.

![Preference-File-Example]({{site.baseurl}}/images/preference_example.jpg)

## Implementation:

#### [Step-1] : Include the Library.

   `'implementation 'com.android.support:preference-v7:27.1.1'`

   version should be the latest.

#### [Step-2] : Build the Preference Layout.

I have hardcoded the strings for easy grasp. It is recommended to define in strings.xml file. For ListPreference, array of entries and values shall be defined in arrays.xml

**arrays.xml**
{% highlight XML %}
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="array_entries">
        <item> Red </item>
        <item> Green </item>
        <item> Blue </item>
    </array>
    <array name="array_entry_values">
        <item> red </item>
        <item> green </item>
        <item> blue </item>
    </array>
</resources>
{% endhighlight %}

**Preferences.xml**
 
 Values are hardcoded here for easy understanding and to avoid lookup.

![Preference-File-XML]({{site.baseurl}}/images/preference_file_xml.jpg)


#### [Step-3] : Build Class and load preference file.

{% highlight JAVA %}
public class SettingsFragment extends PreferenceFragmentCompat 
{
    @Override
    public void onCreatePreferences(Bundle savedInstanceState, String rootKey)
    {
        addPreferencesFromResource(R.xml.preferences);
    }
}
{% endhighlight %}

#### [Step-4] : Include fragment in Settings Activity Layout.

{% highlight XML %}
<?xml version="1.0" encoding="utf-8"?>
<fragment
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/settings_fragment"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:name="www.androidcitizen.com.preferencesexample.SettingsFragment"/>
{% endhighlight %}

#### [Step-5] : Include preferenceTheme in styles.xml
{% highlight XML %}
<item name="preferenceTheme"> @style/PreferenceThemeOverlay.v14.Material </item>
{% endhighlight %}

Hope this post helped.

[Part 2][preferencebasic-2] is continuation to this post.

If this post helped or have queries please leave ur comments and share.

[preferencebasic-1]: https//www.androidcitizen.com/shared-preferences-part-1/
[preferencebasic-2]: https://www.androidcitizen.com/shared-preferences-part-2/
[SharedPreferences-Code]: https://github.com/sayaMahi/SharedPreferenceExample

