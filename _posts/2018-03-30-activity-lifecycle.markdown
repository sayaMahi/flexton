---
layout: post
title: Lifecycle of an Activity.
date: 2018-03-30 23:59:20 +0530
description: Activity and Its Lifecycle States. # Add post description (optional)
image:  lifecycle1.jpg # Add image post (optional)
tags: [Android, Activity, Lifecycle]
---


## Whats an Activity

Anything the User can See and Interact with is called User Interface (UI) or Graphical User Interface (GUI).

View is a rectanlge on a screen that shows some content. Content could
be an Image, Text, Button that the app can display. 

A widget term is also similar. In general, It is a control element in a graphical user interface – an element of interaction, such as a button or a scroll bar.

Group of Views/Widgets form an Activity.

Once an Activity is displayed on a device, User interacts and navigates in different ways. User might do many actions. User might open a new app, close the app, clik on home button, open a different app, click on recents button, re-open an app from overview screen, User might watch a Video by rotating the device.

To handle the limited resources effectively, and to re-open the apps gracefully, Android Framework introduces different States for an Activity, based on if the Activity is on the Forground or in the Background or somewhere in between.

If an App has many Activities, and as you keep navigating through them, new screen comes on top, and previous screen goes into background, like a stack of cards.

If an Activity (UI Screen) is Visible and Active, it  means that activity is on the Foreground. It means, it is on the top of Activity Stack. Active meaning, UI has focus, where User can enter something or Click on something.

## Activity Lifecycle States

![Lifecycle1]({{site.baseurl}}/images/lifecycle1.jpg)

To easily grasp different States of an Activity and their Order of occurrence, lets consider Music Player example which has Open, Play, Resume, Pause, Stop, Cancel states/functionalities. With out Mapping the States of Player to States of an Activity Literally, lets get the general idea.

A song continues (resumes) from where its stopped earlier, if it has been played and app is not closed. So basically, it looks for any data, and Resumes from there, else Plays from start.  

User Listens Song from a Speaker which is shared by even Caller App. So, if we get a Call while listening, song gets Paused, and when Call ends,  Song gets Resumed. Finally you could Close/Exit the app.
You have like, Start->Resume->Play->Pause->Stop->Destroy(Exit)

Similarly, we have a Display which is shared by different apps or different Activities of same app. When another App/Activity comes on top, previous Activity gets Paused/Stopped.

onRestart() method is called after onStop() if the user is re-opening the App from launcher or recents (overview) screen.

When the User rotates the screen, Android Framework is informed that the screen configuration is changed, so it thinks there is better layout that can show contents in a better way using different resources. So, the existing lifecycle gets stopped/destroyed and fresh cycle starts. 

Incase if the user is doing memory intensive tasks, Android will kill activities on the background to allot resources for the new App.

## Different User Actions vs States:

![Lifecycle2]({{site.baseurl}}/images/lifecycle2.jpg)


References: Udacity, Android Developer Document, Code Path, StackOverflow.