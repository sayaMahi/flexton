---
layout: post
title: "RecyclerView with CustomAdapter Example."
date: 2018-03-14 20:20:20 +0530
description:  RecyclerView, differences with ListView, Example. 
image: recyclerviewstamp.jpg # Add image post (optional)
tags: [Android, AdapterView, RecyclerView, CustomAdapter, ViewHolder]
---

Check [this][adapterbasic-1] link to understand steps involved for Adapters. 

I recommend above link, to grasp the steps better.

Check [this][adapterbasic-2] link to understand ListView Example.

This post is in continuation to ListView CustomAdapter example, as we shall learn
through differences between ListView/RecyclerView. 


## RecyclerView
We could say is an efficient approach of ListView. 

Recommended practices of ListView were made manadatory in an 
efficient way in RecyclerView. Like, `ViewHolder`, `Adapter`.

## RecyclerView-vs-ListView

ListView is part of native api. 
>RecyclerView is part of `support Library`

If you were to dynamically Add/Remove an Item in a ListView, 
for Animating those changes it could get very complex. 
>In RecyclerView, `Item Animations` were added to address that.

In ListView, divider were simple. Adding custom divider is complex.
>In RecyclerView, `ItemDecoration` were added to customise the 
divider/seperators between Items.

In ListView, Scrolling is Vertical.
>In RecyclerView, you can have Vertical/Horizontal Scrolling effect. 
`LayoutManager` takes care of placing the items in the desired view.

Just like ListView, we additionally had GridView, Spinner.
>In RecyclerView, we have LinearLayoutManager, GridLayoutManager, 
StaggeredLayoutManager(for items of variying sizes). 


Below is the example that we will be working on.

![ListViewAndAdapterExample1]({{site.baseurl}}/images/recyclerviewexample.jpg)

### Steps:

* Add RecyclerView Support library in gradle.

* layout with recyclerView widget.

* For view Item, Define list_item.xml 

* Define a new Data Model (Data Source). (DataModel.java)

* View Holder (as below)

```
In ListView, getView() function (see the recommended) is made manadatory as 
ViewHolder in RecyclerView.

For RecyclerView,

* class DataViewHolder extends RecyclerView.ViewHolder

    1. Constructor method.
        - findViewById of all the items in list_item.xml
    2. method to set values to views
        - onBindViewHolder calls this function.

```

* Adapter (as below)

```
In List View, we extend CustomAdapter from ArrayAdapter or BaseAdapter.

For RecyclerView,

* DataModelAdapter.java extends RecyclerView.Adapter<DataAdapter.DataViewHolder>

    Override Methods:
    1. onCreateViewHolder
        - inflate list item, and get view handle.
        - instance of ViewHolder and pass View handle.
    2. onBindViewHolder
        - setting the data to items. (usually call a method in ViewHolder)
    3. getItemCount
        - total number of items.
```

* Create instance of Adapter and attach it to RecyclerView.

```
Aditionally, instead of ListView taking care of presenting the data,
in RecyclerView, LayoutManager will take care of presenting.

* Create instance of LayoutManager and attach it to RecyclerView.
```

Check <a href="http://developer.android.com/reference/android/support/v7/widget/RecyclerView.html" 
target="_blank">this</a> to see about RecyclerView class.

**Total Files:**

|------------------------------------------|
| XML Files         |    Java Files        |
|------------------------------------------|
| MainActivity.xml  | MainActivity.java    |
| SecondActivity.xml| SecondActivity.java  |
|    |  |
| list_item.xml     | DataModel.java |
|                   | DataAdapter.java|
| res File | |
| image.png||
|------------------------------------------|


### XML Files:

![ListViewAndAdapterExample2]({{site.baseurl}}/images/recyclerviewxml.jpg)

### Java Files:

![ListViewAndAdapterExample3]({{site.baseurl}}/images/recyclerviewjava.jpg)


source code <a href="http://github.com/sayaMahi/RecyclerView-Basic-example" target="_blank">here</a>


## activity_main.xml

{% highlight XML %}

    <TextView
        android:id="@+id/..."
        android:layout_height="@dimen/list_item_height"
        android:layout_width="match_parent"
        style="@style/CategoryStyle"
        android:background="@color/..."
        android:text="..."
        android:layout_margin="16dp"
        android:gravity="center_vertical"
        android:layout_gravity="center_vertical"/>

{% endhighlight %}


## list_item.xml

height is hardcoded, only to tell its important to have fixed height value 
else Adapter might show error while calculating the required height dp value for each list item.

{% highlight XML %}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="horizontal"
    android:layout_width="wrap_parent"
    android:layout_height="wrap_parent">
    
    <ImageView
        android:id="@+id/imageView"
        android:layout_width="88dp"
        android:layout_height="88dp"
        android:src="@mipmap/ic_launcher"/>
    
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="88dp"
        android:orientation="vertical"
        android:paddingLeft="16dp">

        <TextView
            android:id="@+id/titleView"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1"
            android:gravity="bottom"
            android:text="Title"/>

        <TextView
            android:id="@+id/descriptionView"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1"
            android:gravity="bottom"
            android:text="Description"/>

    </LinearLayout>

</LinearLayout>
{% endhighlight %}


## activity_second.xml

{% highlight XML %}
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.RecyclerView 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/recycleview2"
    android:background="#FFFFFF"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

</android.support.v7.widget.RecyclerView>
{% endhighlight %}

## MainActivity.java:
{% highlight Java %}
TextView tv = (TextView) findViewById(R.id.(...id name...));
tv.setOnClickListener(new View.OnClickListener() {
  @Override
  public void onClick(View v) {
  Intent intent = new Intent(MainActivity.this, xxxActivity.class);      
  startActivity(intent);
 }
});
{% endhighlight %}

## SecondActivity.java:
{% highlight Java %}
//Data Source
ArrayList<DataModel> items = new ArrayList<DataModel>();

items.add(new DataModel("Android 8.1", "Called Oreo", R.drawable.pngImage));
items.add(new DataModel("Android 8", "Called Oreo", R.drawable.pngImage));
items.add(new DataModel("Android 7.1", "Called Nougat", R.drawable.pngImage));
items.add(new DataModel("Android 7", "Called Nougat", R.drawable.pngImage));

...


items.add(new DataModel("Android 2.3", "Called Gingerbread", R.drawable.pngImage));


LinearLayoutManager lvManager = new LinearLayoutManager(this);
RecyclerView recycleview2 = (RecyclerView) findViewById(R.id.recycleview2);
recycleview2.setLayoutManager(lvManager);
recycleview2.setHasFixedSize(true);

DataAdapter itemsAdapter = new DataAdapter(items);
recycleview2.setAdapter(itemsAdapter);

{% endhighlight %}

## DataModel.java:
{% highlight Java %}

This has same code as DataModel.java from previous example.

{% endhighlight %}

## DataAdapter.java:

{% highlight Java %}
public class DataAdapter 
            extends RecyclerView.Adapter<DataAdapter.DataViewHolder> {
    ArrayList<DataModel> mData;
    int mNumofItems;

    public DataAdapter(ArrayList<DataModel> mData) {
        this.mData = mData;
        mNumofItems = mData.size();
    }

    @Override
    public DataViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        //return null;
        View view = LayoutInflater.from(parent.getContext())
                    .inflate(R.layout.list_item, parent, false);

        DataViewHolder viewHolder = new DataViewHolder(view);

        return viewHolder;
    }

    @Override
    public void onBindViewHolder(DataViewHolder holder, int position) {
        holder.onBind(position);
    }

    @Override
    public int getItemCount() {
        return mNumofItems;
    }

    class DataViewHolder  extends RecyclerView.ViewHolder {
        ImageView mImageView;
        TextView mTxtView;
        TextView mDescView;

        public DataViewHolder(View itemView) {
            super(itemView);
            mImageView = (ImageView) itemView.findViewById(R.id.imageView);
            mTxtView = (TextView) itemView.findViewById(R.id.titleView);
            mDescView = (TextView) itemView.findViewById(R.id.descriptionView);
        }

        void onBind(int position){
            mImageView.setImageResource(mData.get(position).getiImageId());
            mTxtView.setText(mData.get(position).getmTitle());
            mDescView.setText(mData.get(position).getmDescription());
        }
    }
}
{% endhighlight %}

References: Udacity, Android Developer Document, Code Path, StackOverflow.


[adapterbasic-1]: http://www.androidcitizen.com/listview-with-arrayadapter/
[adapterbasic-2]: http://www.androidcitizen.com/example-listview-with-customadapter/
[arraydapterapi-1]: http://developer.android.com/reference/android/widget/ArrayAdapter.html
