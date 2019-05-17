---
layout: post
title: Android Development ---- Pick Up Picture From Gallery
categories: [Android Development]
tags: [Android, Java]
fullview: false
comments: false
shortinfo: This document is to demonstrate one function in Android Development -- Pick Up Picture from the gallery. This is the first time write about android development, so I would like to illustrate the interface design and interface logic too.
---
#### Problem Description
This document is to demonstrate one function in Android Development -- Pick Up Picture from the
gallery. This is the first time write about android development, so I would like to illustrate
the interface design and interface logic too.

[YouTube Link](https://youtu.be/z3IAjcwEAb8)

#### Tool
I used Android Studio to develop my app. It is a mature technology that helps me to build app easily.

#### Create UI
First we need have a UI in this project. The directory of the UI is in the "res" directory. Find the file named "activity_main.xml". This is the first interface when open a app.

![UI Directory](https://raw.githubusercontent.com/scao7/dbyll/gh-pages/assets/media/androidRes/UIDir.PNG)

##### Add ImageView and Button
In the "activity_main.xml" add a Button like this:
```html
<Button
      android:id="@+id/open_picture"
      style="@style/Base.Widget.AppCompat.Button.Colored"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:text="@string/open_picture" />
```
Also add the ImageView:
```html
<ImageView
       android:id="@+id/imageView"
       android:layout_width="match_parent"
       android:layout_height="500dp"
       android:scaleType="fitCenter"
       android:src="@drawable/sample" />
```
#### Implement the interface logic and Pick Up Logic
Go to Java directory find the "MainActivity" class to implement the Logic
First we need define two different code indicate the if the app was granted to open the camera.
```java
private static final int IMAGE_PICK_CODE = 1000;
private static final int PERMISSION_CODE = 1001;
```
We need define a button object called open button and get the UI information on the button object. Use the "findViewById" function to get the object.
```Java
openBtn = findViewById(R.id.open_picture);
```
After we get the button object, we need add a OnClickListener(). We need check the permission code if the permission was granted to the app.
```java
openBtn.setOnClickListener(new View.OnClickListener() {
       @Override
       public void onClick(View v) {
           //check runtime permission
           if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.M){
               if(checkSelfPermission((Manifest.permission.READ_EXTERNAL_STORAGE))
                       == PackageManager.PERMISSION_DENIED){
                       //permission not granted request
                       String[] permissions = {Manifest.permission.READ_EXTERNAL_STORAGE};
                       //show popup for runtime permission
                       requestPermissions(permissions, PERMISSION_CODE);
               }
               else{
                   //permission already granted
                   pickImageFromGallery();
               }
           }
           else{
               //system os is less then marshmallow
               pickImageFromGallery();
           }
       }
   });
```
Notice there is a function called pickImageFromGallery() which is the method that actually pick the image in the gallery. We need create a Intent which is ACTION_PICK intent and set the select type to all the image file. Pass the intent and IMAGE_PICK_CODE into the startActivityForResult() function.

```java
private void pickImageFromGallery(){
       //intent to pick image
       Intent intent = new Intent((Intent.ACTION_PICK));
       intent.setType("image/*");
       startActivityForResult(intent, IMAGE_PICK_CODE);
   }
```
In the first time to open gallery, we need to ask the user if grant the permission to open the gallery.
```java
@Override
public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
    switch (requestCode){
        case PERMISSION_CODE: {
            if(grantResults.length>0 && grantResults[0] == PackageManager.PERMISSION_GRANTED){
                // permission granted
                pickImageFromGallery();
            }
            else{
                // permission denied
                Toast.makeText(this,"Permission denied!",Toast.LENGTH_SHORT).show();
            }

        }
    }
}
```
When the Intent get result and the request is the pick up code. We can set the images URI to the image view to display the image on the screen.
```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    if(resultCode == RESULT_OK && requestCode == IMAGE_PICK_CODE){
        imageView.setImageURI(data.getData());
    }
}
```
The last step is to make sure the cell phone not denied you the first add code to the Manifest file.
```html
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```
