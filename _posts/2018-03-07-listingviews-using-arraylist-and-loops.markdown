---
layout: post
title: "Listing Views: using LinearLayout, ArrayList and Loops"
date: 2018-03-07 10:08:20 +0530
description:  Basic way of Listing Views. 
image:  # Add image post (optional)
tags: [Android, ListView, ArrayList]
---

Consider You have arraylist of type String, that need to be dislayed on the Screen.

{% highlight Java %}
ArrayList<String> numberlist = new ArrayList<String>();

numberlist.add("one");
numberlist.add("two");
numberlist.add("three");
numberlist.add("four");
numberlist.add("five");
numberlist.add("six");
numberlist.add("seven");
numberlist.add("eight");
numberlist.add("nine");
numberlist.add("ten");
{% endhighlight %}

For the Layout XML file, define an ID, lets say 'mainView'. Consider it to be Liner Layout and Oriented Vertically.

In Activity Class,

{% highlight Java %}

LinearLayout mainView = (LinearLayout) findViewById(R.id.mainView);  //Get handle of Parent Layout

Because the ArrayList is of type String, we could define a single TextView.

  for (int indexNumber = 0; indexNumber < numberlist.size(); indexNumber++);
    {
        TextView numView = new TextView(this);          //Define TextView.
        numView.setText(numberlist.get(indexNumber));   //Set Text.
            
        mainView.addView(numView);                      //Add the newly Created View to Parent Layout.
    }

{% endhighlight %}

Here, the disadvantage of this method is Performance and Memory Constraints.

Suppose, instead of 10 items, suppose there are 1000 items defined in the ArrayList, above method creates 1000 Views.

Though, Mobile Display can show only few number of items at Once, and there is possibility that User will not scroll through all the items, creating all the Views before hand is Un-necessary, wastage of memory allocation and will also cause performace issues as you device has very limited memory, and you will find device being slow.

For Example, Contacts app, where the use sees only whats necessary from a pile of huge number of contacts.

To do it in an efficient way, check [ListView With ArrayAdapters][listview-arrayadapter].

[listview-arrayadapter]: http://www.androidcitizen.com/listview-with-arrayadapter/

References: Udacity, Android Developer Document, Code Path, StackOverflow.


