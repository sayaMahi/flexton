---
layout: post
title: "ListView with CustomAdapter Example."
date: 2018-03-12 08:40:20 +0530
description:  ListView and CustomAdapter. 
image: listviewstamp.jpg # Add image post (optional)
tags: [Android, ListView, ArrayAdapter, AdapterView, CustomAdapter]
---

Check [this][adapterbasic-1] link to understand AdapterView and Adapter.

This post is in continuation to below example. 
Check [this][adapterbasic-2] link to understand basic Example.

If we have only 1 View per Item, above example would suffice. 
Incase if you have more than 1 View Item, example, 1 ImageView, 1 or 2 TextViews, then you have to follow below approach.

Below is the example that we will be working on.

![ListViewAndAdapterExample1]({{site.baseurl}}/images/customadapexample.jpg)

Additional changes from the previous example are.

* One New TextView to open a new screen. So, add Click Listener for it.
* New List Item. So Define example list_item.xml (Same XML will be resued for both Screen 2 and Sceen 3)
>(instead of android.R.layout.simple_list_item_1)

* Our Data is not just String. So we need to define a new Data Model.
  (DataModel.java) 
>(intead of array of strings, We shall hold array of this DataModel object)

* DataModelAdapter.java 
> This is our custom Adapter.
  ArrayAdapter that we have used earlier, holds only 1 Object. 
  In the previous example we have 1 item (i.e.,String). Now we have more items,
  so we are defining a Class with required members, and we will use its Object (of new class).

* one image file (image.png) at res/drawabales.

Check <a href="https://developer.android.com/reference/android/widget/ArrayAdapter.html" target="_blank">this</a> to see about ArrayAdapter class.

Total Files:

|------------------------------------------|
| XML Files         |    Java Files        |
|------------------------------------------|
| MainActivity.xml  | MainActivity.java    |
| SecondActivity.xml| SecondActivity.java  |
| ThirdActivity.xml | ThirdActivity.java   |
|    |  |
| list_item.xml     | DataModel.java |
|                   | DataAdapter.java|
| res File | |
| image.png||
|------------------------------------------|


### XML Files:

![ListViewAndAdapterExample2]({{site.baseurl}}/images/customadapexample1.jpg)

### Java Files:

![ListViewAndAdapterExample3]({{site.baseurl}}/images/customadapexample2.jpg)

## activity_main.xml
Add: 
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

height is hardcoded, only to tell its important to have fixed height value. else Adapter might show error while calculating the required height dp value for each list item.

{% highlight XML %}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    
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


## activity_fourth.xml

{% highlight XML %}
<?xml version="1.0" encoding="utf-8"?>
<ListView xmlns:android="...//schemas.android.com/apk/res/android"
    xmlns:app="...//schemas.android.com/apk/res-auto"
    xmlns:tools="...//schemas.android.com/tools"
    android:id="@+id/listview..."
    android:layout_width="match_parent"
    android:layout_height="match_parent">
</ListView>
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

## FourthActivity.java:
{% highlight Java %}
This has same code as SecondActivity.java from previous example
{% endhighlight %}

## DataModel.java:
{% highlight Java %}
public class DataModel {
    private String mTitle;
    private String mDescription;

    private int mImageID = NO_IMAGE;

    private static final int NO_IMAGE = -1;

    public DataModel(String mTitle, String mDescription) {
        this.mTitle = mTitle;
        this.mDescription = mDescription;
    }

    public DataModel(String mTitle, String mDescription, int iImageId) {
        this.mTitle = mTitle;
        this.mDescription = mDescription;
        this.mImageID = iImageId;  //has image
    }

    public boolean hasImage(){
        return mImageID != NO_IMAGE;
    }

    public String getmDescription() {
        return mDescription;
    }

    public String getmTitle() {
        return mTitle;
    }

    public int getiImageId() {
        return mImageID;
    }

}
{% endhighlight %}

## DataAdapter.java:

** NOT RECOMMENDED

{% highlight Java %}
public class DataAdapter extends ArrayAdapter<DataModel> {

    public DataAdapter(Context context, ArrayList<DataModel> objects) {
        super(context, 0, objects);
    }

    @NonNull
    @Override
    public View getView(int position, View convertView, @NonNull ViewGroup parent) {
        View listItem = LayoutInflater.from(getContext()).inflate(
                    R.layout.list_item, parent, false);
        
        ImageView mImageView = (ImageView) listItem.findViewById(R.id.imageView);
        
        DataModel currentItem = getItem(position);

        if(currentItem.hasImage()) {

            mImageView.setVisibility(View.VISIBLE);
            mImageView.setImageResource(currentItem.getiImageId());
        }
        else {
            mImageView.setVisibility(View.GONE);
        }

        TextView mTxtView = (TextView) listItem.findViewById(R.id.titleView);
        mTxtView.setText(currentItem.getmTitle());

        TextView mDescView = (TextView) listItem.findViewById(R.id.descriptionView);
        mDescView.setText(currentItem.getmDescription());

        return listItem;
    }
 
}
{% endhighlight %}

> Problem with the above method is, if there are 1000 items to be displayed, there will 
be 1000 views that will be created. But very few items can fit on Visible screen at a 
time. So, the above procedure is memory intensive. 

> To make it efficient, we shall hold the views in a ViewHolder, and re-use the items, 
fill it with new data as the user scrolls the screens.

> Please Note, ViewHolder is only RECOMMENDED Practice in ListView. But it is Mandate 
for RecycleView.

## DataAdapter.java:

** RECOMMENDED

{% highlight Java %}

static class ViewHolder{
    ImageView mImageView;
    TextView mTxtView;
    TextView mDescView;
}

public class DataAdapter extends ArrayAdapter<DataModel> {

    public DataAdapter(Context context, ArrayList<DataModel> objects) {
        super(context, 0, objects);
    }

    @NonNull
    @Override
    public View getView(int position, View convertView, @NonNull ViewGroup parent) {

        View listItem = convertView;
        ViewHolder holder = null;

        if(listItem == null){
            listItem = LayoutInflater.from(getContext()).inflate(
                    R.layout.list_item, parent, false);
            
            holder = new ViewHolder();
        
            holder.mImageView = (ImageView) listItem.findViewById(R.id.imageView);
            holder.mTxtView = (TextView) listItem.findViewById(R.id.titleView);
            holder.mDescView = (TextView) listItem.findViewById(R.id.descriptionView);

            vi.setTag(holder);
        }
        else {
            holder = (ViewHolder) vi.getTag();
        }

        DataModel currentItem = getItem(position);
        if(currentItem.hasImage()) {

            holder.mImageView.setVisibility(View.VISIBLE);
            holder.mImageView.setImageResource(currentItem.getiImageId());
        }
        else {
            holder.mImageView.setVisibility(View.GONE);
        }

        
        holder.mTxtView.setText(currentItem.getmTitle());

        
        holder.mDescView.setText(currentItem.getmDescription());

        return listItem;
    
}
{% endhighlight %}
References: Udacity, Android Developer Document, Code Path, StackOverflow.


[adapterbasic-1]: https://www.androidcitizen.com/listview-with-arrayadapter/
[adapterbasic-2]: https://www.androidcitizen.com/example-listview-with-arrayadapter/
[arraydapterapi-1]: https://developer.android.com/reference/android/widget/ArrayAdapter.html
