---
layout: post
title: Android Development ---- Pick Up Picture From Gallery
categories: [Android Development]
tags: [Android, Java]
fullview: false
comments: false
---
#### Problem Description
This document is to demonstrate one function in Android Development -- Pick Up Picture from the
gallery. This is the first time write about android development, so I would like to illustrate
the interface design and interface logic too.


#### Tool
I used Android Studio to develop my app. It is a mature technology that helps me to build app easily.

#### Create UI
First we need have a UI in this project. The directory of the UI is in the "res" directory. Find the file named "activity_main.xml". This is the first interface when open a app.

![UI Directory](https://raw.githubusercontent.com/scao7/dbyll/gh-pages/assets/media/androidRes/UIDir.PNG)

##### Add ImageView and Button
In the "activity_main.xml" add a Button like this:
```HTML
<Button
      android:id="@+id/open_picture"
      style="@style/Base.Widget.AppCompat.Button.Colored"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:text="@string/open_picture" />
```
Also add the ImageView:
```
<ImageView
       android:id="@+id/imageView"
       android:layout_width="match_parent"
       android:layout_height="500dp"
       android:scaleType="fitCenter"
       android:src="@drawable/sample" />
```
#### Implement the interface logic and Pick Up Logic
Go to directory find the "MainActivity" class
