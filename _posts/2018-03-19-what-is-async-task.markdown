---
layout: post
title: "What is Async Task ? "
date: 2018-03-19 19:20:20 +0530
description:  Async Task in Android. 
image:  asyncbanner.jpg # Add image post (optional)
tags: [Android, Async Task, doInBackground]
---

Have you come across, **ANR** (Application Not Responding) Error like below.

![ANR1]({{site.baseurl}}/images/asyncanr.jpg)

Well one of main reason could be your Application **Main Thread/UI Thread** is running for longer time. 

In Android, application responsiveness is monitored by the Activity Manager and Window Manager system services. 

Android will display the ANR dialog for a particular application when it detects one of the following conditions:

* No response to an input event within 5 seconds.

* A BroadcastReceiver hasn't finished executing within 10 seconds.

## UI/Main Thread

Android Applications usually run in a single thread by default. Its the UI Thread, also called Main Thread.

Tasks running in Main Thread, execute One after One.

Usually any method/task that runs in Main thread should run 
for very less time. (We are talking about Nano-Milli Seconds here).

Some of the Tasks that run in Main Thread:

* System Events
* Input Events
* Application Callbacks
* Services
* Alaram
* UI Rendering

If a task in Main Thread wants to fetch some data from a URL or from Database,
lets say, which usually takes considerably more time, holds next event from executing untill fetch completes, (similarly for any Long lasting operations), which triggers ANR error, like below.


![ANR1]({{site.baseurl}}/images/asyncwrongway.jpg)

Any operation which are time consuming should run in a seperate Thread. (background or worker thread.)

But, you can update UI elements ONLY from Main Thread. You will get an exception
if you try to update UI from a seperate Thread.

To Fix this, Android provides few methods to Acess Main Thread from other Threads.

Like, 
 1. Activity.runOnUiThread(Runnable)
 2. View.post(Runnable)
 3. View.postDelayed(Runnable, long)

 For complex interactions, Handler method can be used.

### Concurrent Processing

Meaning Different Tasks running at same time.

Android provides few Classes to achive this.

1. AsyncTask
> To get task done On/Off the Main Thread.
2. HandlerThread
> Dedicated Thread for API Callbacks
3. ThreadPool
> Running many threads at once.
4. IntentService
> To send Intents from UI Thread, and to run background services.

![ANR1]({{site.baseurl}}/images/asyncrightway.jpg)

## AsyncTask ? When to Use ?

<a href="http://developer.android.com/reference/android/os/AsyncTask.html" target="_blank">AsyncTask</a> is not a Thread but a special Class where it should ideablly be used for short operations (few seconds). 

It allows you to perform background operations.

It can also publish results on the UI thread.

Threads also can be used to perform background operations and update the UI thread. But we need to take care of synchronizing with the main thread.


## How to use AsyncTask

MyClass extends AsyncTask <TypeOfVarArgParams, ProgressValue, ResultValue>

It has 4 Methods.

1. onPreExecute()
> * Runs on `Main Thread`. 
> * This is invoked before starting background thread.

2. doInBackground(Params...)
> * Runs on `Background Thread`. 
> * Example: your HTTP Call executes here.
> * Called after onPreExecute().
> * This can call publishProgress(Progress...).

3. onProgressUpdate(Progress...)
> * Runs on `Main Thread`. 
> * This is invoked if after publishProgress(Progress...)
> * Used to update Progress Bar Value.

4. onPostExecute(Result)
> * Runs on `Main Thread`. 
> * synchronizes itself UI
> * This is invoked if after doInBackground() finishes, and its result is passed here.
> * You may update UI now with the result passed.

## How to Run.
MyClass.execute(...);

## How to Cancel.
MyClass.cancel(true);

{% highlight JAVA %}
protected Object doInBackground(Object... x) 
{
    while (...) {
      if (isCancelled()) break;
    }
    return null;
}
{% endhighlight %}

## Challenges:

If you rotate your device, lifecycle states kick in. Android may kill the Activity, restart it. There could be a Background Thread running at the same time, and there could be memory leak or an Exception.

This we shall see in later post.

References: Udacity, Android Developer Document, Code Path, StackOverflow.


