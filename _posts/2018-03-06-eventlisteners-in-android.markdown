---
layout: post
title: "EventListeners: onClickListeners, onClick"
date: 2018-03-06 13:50:20 +0530
description:  Event Listeners in Android. 
image:  eventlisteners.jpg 
tags: [Android, onClickListener, onClick, Event Listeners]
---
In our real word, consider different type of protection forces (City, Airport, Railway, Border etc). If some crime/event happens specific type of force are notified, and the type of action that is taken differs from event-event.

Here, protection forces are Event Listeners.

Similary,

Consider we have Screen 1 presented to a User. User may start interacting by touching/clicking the View items (i.e., Button/TexView/any). User could also  double touch, long touch. These Clicks are events that are notified (by the Screen Hardware) to the Android Framework, and Android will take of detecting from which View these events are generated. As a developer, if you want to perform some action on a View when any of these events occur, you could do so by using Interfaces, called Event Listeners.

![ViewCLickListener]({{site.baseurl}}/images/viewclick.jpg)

Views have a collection of Interfaces with callback functions, (functions that are called by the Framework when an Event occurs) which developer can use to define custom actions.

>Some of the Interfaces that are available are

>* View.OnClickListener
>* View.OnLongClickListener
>* View.OnFocusChangeListener
>* View.OnKeyListener
>* View.OnTouchListener
>* View.OnCreateContextMenuListener 

You Can Create Event Listener in 2 ways. 

1. through XML
2. through Code.

## through XML 
>* Declare function name in XML View.
>* Define function in Activity.

{% highlight XML %}
android:onClick="myCustomeListener" // .XML layout file
{% endhighlight %}

{% highlight java %}
public void myCustomListener(View view){ // .class Activity file
    customBehaviour();
}
{% endhighlight %}

## through Code 
>* Set View ID in XML.
>* Get View handle by findViewById.
>* Create CustomClickListener by inheriting View.OnClickListener Listener.
>* Create Listener Object. 
>* Attach Listener Object to View.

#### ***Method 1***####

{% highlight java %}
TextView tv1 = (TextView) findViewById(R.id.screen2Item);

CustomClickListener secClickList = new CustomClickListener(SecondActivity.class);
tv1.setOnClickListener(secClickList);
		
//CustomClickListener.class

public class CustomClickListener implements View.OnClickListener {

 @Override
 public void onClick(View v) {
 Intent intent = new Intent(v.getContext(), SecondScreenActivity.class);
 v.getContext().startActivity(intent);
 }

 //SecondScreenActivity.class
  setContentView(R.layout.activity_second); // This will inflate/Show the Activity.

{% endhighlight %}

If you have more than one View to Click and more than one Activity to display, you could modify CustomClickListener.class.

#### ***Method 2***####
{% highlight java %}

TextView tv1 = (TextView) findViewById(R.id.screen2Item);
TextView tv2 = (TextView) findViewById(R.id.screen3Item);

CustomClickListener secClickList = new CustomClickListener(SecondScreenActivity.class);
tv1.setOnClickListener(secClickList);

CustomClickListener secClickList = new CustomClickListener(ThirdScreenActivity.class);
tv2.setOnClickListener(secClickList);
		
//CustomClickListener.class

public class CustomClickListener implements View.OnClickListener {
    Class<?> clss;

    public SecondScreenClickListener(Class<?> clss) {
        this.clss = clss;
    }

    @Override
    public void onClick(View v) {
        Intent intent = new Intent(v.getContext(), clss);
        v.getContext().startActivity(intent);
    }

//xxxxScreenActivity.class

setContentView(R.layout.activity_second); // This will inflate/Show the Activity.

{% endhighlight %}

#### ***Method 3***####

TextView tv1 = (TextView) findViewById(R.id.screen2Item);

To make it much cleaner, 

{% highlight java %}

CustomClickListener secClickList = new CustomClickListener(SecondActivity.class);
tv1.setOnClickListener(secClickList);
{% endhighlight %}

can be written as, 

{% highlight java %}

secClickList in the above line, can be replaced directly.
// i.e., tv1.setOnClickListener(new CustomClickListener());
//Here CustomClickListener implements View.OnClickListener, which can be used directly.

tv1.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        Intent intent = new Intent(this, SecondActivity.class);
        startActivity(intent);
    }
});

and delete CustomClickListener.class

{% endhighlight %}
 
When to use Code: If you use Fragments. If a View has to call for different funciton at run time.

When to use XML: If you do not re-use the View(s) for more than one behaviour. 

Other difference is explained <a href="http://stackoverflow.com/questions/8977212/button-click-listeners-in-android" target="_blank">here</a> 

Usage of Class<?> can be found <a href="http://stackoverflow.com/questions/9921676/what-does-class-mean-in-java" target="_blank">here</a> 

References: Udacity, Android Developer Document, Code Path, StackOverflow.


