---
layout: post
title: "Understanding Adapters and AdapterViews."
date: 2018-03-08 17:18:20 +0530
description:  Adapters and AdapterViews. 
image: listviewadapter.jpg
tags: [Android, ListView, Adapter, AdapterView]
---
As per API Documentation,

* An **AdapterView** is a view whose children are determined by an Adapter.

  AdapterViews Like:
   * ListView
   * GridView
   * Spinner

* An **Adapter** acts as a bridge between an AdapterView and the underlying data for that view. The Adapter provides access to the data items. The Adapter is also responsible for making a View for each item in the data set.
	
   Like:
    * ArrayAdapter   (if data is String Object)
    * CursorAdapter  (if data is from Database)
    * CustomAdapter  (which extends ArrayAdapter with Custom Object)

To understand Steps involved, for Adapter and AdapterView, lets understand how serving food works in a Restaurant.

(We shall consider ListView for AdapterView)

>Initially you have a Table (number) with no plates. 

* ListView (AdapterView) in XML Layout and its ID.

>Customer orders your food to Bearer.  
>Bearer picks food which he must have got from a Data Source. (Stored-Food-from-Fridge or Fresh-from-Market orAmazon-Store).
>Bearer fill the food on to the Plate.

* ListView talks to Adapter.
* Adaper Collects data from DataSource 
	* (Data Source can be Array, ArrayList, JSON/REST, SQL.) 
* Adapter fills Views with data.

>Bearer puts the Plate (of food) on your table.

* Setting the Adapter to ListView.

> Just as Plates can be of different types, there are different AdapterView like ListView, GridView, Spinner.
> Just like different food items can be grouped together on a plate, different Views can be grouped together in a ViewGroup to make an element (List Item).

>Here, 
>Plate is the ListView(AdapterView). 
>Bearer is the Adaper. 
>DataSource is SQL, Array, ArrayList, JSON.
>Item View can be a String,Image,Button or any View or Combination.

Imagine a person on the table leaves, and another person wants to occupy the chair, then we have the option to Put a new plate or Clean the used plate and fill the food items.

Exactly, when the user scrolls the list up/down, in order to Populate new data, Adapter can create a new View, set the data and return to the AdapterView, or Re-Use an existing View, set new data and return the View to AdapterView.

Next: Check here for Simple [Example][Example-1]

Below image explains how this works.

![ListViewAndAdapter]({{site.baseurl}}/images/listviewadaptervisualize.jpg)

References: Udacity, Android Developer Document, Code Path, StackOverflow.

[Example-1]: http://www.androidcitizen.com/example-listview-with-arrayadapter/

