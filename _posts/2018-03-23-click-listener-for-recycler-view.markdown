---
layout: post
title: Item Click Listener for RecyclerView.
date: 2018-03-23 23:59:20 +0530
description: Item Click Listener for RecyclerView # Add post description (optional)
image:  clicklistenerrecyclerviewbanner.jpg # Add image post (optional)
tags: [Android, Click Listener, Item Click Listener]
---

RecyclerView `Do Not` have `setOnItemClickListener`, like ListView/View does.

This can be achieved by using Interface.

This post is incontinuation to this <a href="https://runningnotes.info/example-recyclerview-with-customadapter/" target="_blank">RecyclerView Example</a>. 

Before starting any further, Lets have a recap of Interface.

## Interface Features

Some of the features(that we shall be using) of an Interface are: 

1. You can not instantiate a Variable, nor create an Object of an Interface.
2. Methods are public, abstract. (No Implementation)
3. An Interface reference variable can point to objects of its implementing classes
4. Interface variable must be initialized, else the compiler will throw an error.

Steps correspond below image.

![Interface1]({{site.baseurl}}/images/interface1.jpg)

Assigning an Implementing Class Object to Interface Variable will be used in this example. This is mainly useful incase, if the Object changes in future, there wont be any other code changes.

## Steps Overview

If you look at this <a href="https://runningnotes.info/eventlisteners-in-android/" target="_blank">Click Listener Example</a>, steps involved to create Click Listener are,

1. Implementation of View.OnClickListener 
2. viewItem.setOnClickListener(Object);

Below example is in continuation to RecyclerView Example, so have a look at this <a href="https://runningnotes.info/example-recyclerview-with-customadapter/" target="_blank">RecyclerView Example</a>.

Here, we start with Creating an Interface and an abstract Method.

Create an Interface Variable and Initialize in the Constructor.

Now Implement View.OnClickLisener in the View Holder and Overide OnClick Method, call interface method within.

In the View Holder Constructor, setOnClickListener to the View Item after getting the ID. 

From the Activity, Implement the new Interface that is created, and 
define the abstract method for the required behavior needed when 
Item gets Clicked. Pass this object to initialize the Interface variable.


![Interface2]({{site.baseurl}}/images/interface2.jpg)

## Overview of Code:

![Interface3]({{site.baseurl}}/images/interface3.jpg)


References: Udacity, Android Developer Document, Code Path, StackOverflow.