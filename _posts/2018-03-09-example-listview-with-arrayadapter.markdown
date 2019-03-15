---
layout: post
title: "ListView with ArrayAdapter Example."
date: 2018-03-09 17:18:20 +0530
description:  ListView and ArrayAdapter.
image: adapterview.jpg # Add image post (optional)
tags: [Android, ListView, ArrayAdapter, AdapterView]
---

Check [this][adapterbasic-1] link to understand AdapterView and Adapter.

If you are done with the post, Check [this][adapterbasic-2] link for Creating Custom Adapter Example.

Below is the example that we will be working on.

![ListViewAndAdapterExample1]({{site.baseurl}}/images/listviewadapterexample.jpg)

Files to be Created to acheive this is as below.

|------------------------------------------|
| XML Files         |    Java Files        |
|------------------------------------------|
| MainActivity.xml  | MainActivity.java    |
| SecondActivity.xml| SecondActivity.java  |
| ThirdActivity.xml | ThirdActivity.java   |
|------------------------------------------|

ListViewItem.xml(Optional). 
Here in the below example we are not creating this XML file. Instead, we 
are using android.R.layout.simple_list_item_1 provided by Android framework.
Look for this while creating ArrayAdapter in SecondActivity.java / ThirdActivity.java.

Other XML files are dimens.xml, colors.xml, 
styles.xml which you could segregate. I will not be covering this.

### XML Files:

![ListViewAndAdapterExample2]({{site.baseurl}}/images/listviewadapterlayoutexample1.jpg)

### Java Files:

![ListViewAndAdapterExample3]({{site.baseurl}}/images/listviewadapterlayoutexample2.jpg)

## activity_main.xml

{% highlight XML %}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FFF7DA"
    android:orientation="vertical">

     <TextView
        android:id="@+id/screen2AdapterView"
        android:layout_height="@dimen/list_item_height"
        android:layout_width="match_parent"
        android:text="Screen 2"
        android:padding="16dp"
        android:gravity="center_vertical"
        android:textColor="#FFFFFF"
        android:background="#29A921"
        android:textStyle="bold"
        android:textAppearance="?android:textAppearanceMedium"
        android:layout_margin="16dp"
        android:layout_gravity="center"/>

     <!-- Common Attributes can be seperated at values/styles.xml; 
     and attributes like color/text can be seperated at values/colors.xml, 
     values/strings.xml, values/dimens.xml
     As Below -->   

    <TextView
        android:id="@+id/screen3AdapterView"
        android:layout_height="@dimen/list_item_height"
        android:layout_width="match_parent"
        style="@style/CategoryStyle"
        android:background="@color/screen3"
        android:text="Screen 3"
        android:layout_margin="16dp"
        android:gravity="center_vertical"
        android:layout_gravity="center_vertical"/>
</LinearLayout>
{% endhighlight %}

## activity_second.xml

{% highlight XML %}
<?xml version="1.0" encoding="utf-8"?>
<ListView xmlns:android="...//schemas.android.com/apk/res/android"
    xmlns:app="...//schemas.android.com/apk/res-auto"
    xmlns:tools="...//schemas.android.com/tools"
    android:id="@+id/listview2"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
</ListView>
{% endhighlight %}

## activity_third.xml

{% highlight XML %}
<?xml version="1.0" encoding="utf-8"?>
<ListView xmlns:android="...//schemas.android.com/apk/res/android"
    xmlns:app="...//schemas.android.com/apk/res-auto"
    xmlns:tools="...//schemas.android.com/tools"
    android:id="@+id/listview3"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
</ListView>
{% endhighlight %}
## MainActivity.java:

{% highlight Java %}
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        TextView tv1 = (TextView) findViewById(R.id.screen2AdapterView);
        TextView tv2 = (TextView) findViewById(R.id.screen3AdapterView);

        tv1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent s2intent = new Intent(MainActivity.this, SecondActivity.class);
                startActivity(s2intent);
            }
        });

        tv2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent s3intent = new Intent(MainActivity.this, ThirdActivity.class);
                startActivity(s3intent);
            }
        });

    }
}
{% endhighlight %}

## SecondActivity.java:

{% highlight Java %}
public class SecondActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);

        //Data Source
        ArrayList<String> items = new ArrayList<String>();
        items.add("One");
        items.add("Two");
        items.add("Three");
        items.add("Four");
        items.add("Five");
        items.add("Six");
        items.add("Seven");
        items.add("Eight");
        items.add("Nine");
        items.add("Ten");
        items.add("Eleven");
        items.add("Twelve");
        items.add("Thirteen");
        items.add("Fourteen");
        items.add("Fifteen");

        //AraayAdapter Instance.
        ArrayAdapter<String> itemsAdapter = 
        new ArrayAdapter<String>(this, android.R.layout.simple_list_item_1, items);

        ListView lvScreen2 = (ListView) findViewById(R.id.listview2);

        lvScreen2.setAdapter(itemsAdapter);


    }
}
{% endhighlight %}

## ThirdActivity.java:

{% highlight Java %}
public class ThirdActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_third);

        //Data Source
        ArrayList<String> items = new ArrayList<String>();
        items.add("Alpha");
        items.add("Bravo");
        items.add("Charlie");
        items.add("Delta");
        items.add("Echo");
        items.add("Foxtrot");
        items.add("Golf");
        items.add("Hotel");
        items.add("India");
        items.add("Juliet");
        items.add("Kilo");
        items.add("Lima");
        items.add("Mike");
        items.add("November");
        items.add("Oscar");
        items.add("Papa");
        items.add("Quebec");
        items.add("Romeo");

        //AraayAdapter Instance.
        ArrayAdapter<String> itemsAdapter = 
        new ArrayAdapter<String>(this, android.R.layout.simple_list_item_1, items);

        ListView lvScreen3 = (ListView) findViewById(R.id.listview3);

        lvScreen3.setAdapter(itemsAdapter);
    }
}
{% endhighlight %}

Hope this helped.

Check [this][adapterbasic-2] link for Creating Custom Adapter Example.

References: Udacity, Android Developer Document, Code Path, StackOverflow.


[adapterbasic-1]: http://www.androidcitizen.com/listview-with-arrayadapter/

[adapterbasic-2]: http://www.androidcitizen.com/example-listview-with-customadapter/
