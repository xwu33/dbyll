---
layout: post
title: Android Development ---- Import OpenCV
categories: [Android Development]
tags: [Android, Java, Image Processing]
fullview: false
comments: false
shortinfo: From previews blog, I have the method to do the Ostu thresholding individually. This blog is going to talk about an easy way to do Ostu thresholding which is rely on the OpenCV library. I will demonstrate how to import the OpenCV library in Android and how to apply it to image Processing.
---
Check YouTube link with the video
[YouTube link]()

#### Download OpenCV SDK
[OpenCV Release Page](https://opencv.org/releases/)

#### Import Module
```
File --> New -> Import Module
```
Import the java fold
![pic2](https://raw.githubusercontent.com/scao7/dbyll/gh-pages/assets/media/androidRes/pic2.PNG)

#### Compare the configure information
Make sure the  minSdkVersion and targetSdkVersion matches

![pic3](https://raw.githubusercontent.com/scao7/dbyll/gh-pages/assets/media/androidRes/pic3.PNG)

![pic4](https://raw.githubusercontent.com/scao7/dbyll/gh-pages/assets/media/androidRes/pic4.PNG)

#### Add dependency

```
File --> Project Structure --> App --> Add dependency --> OpenCV
```
#### Copy library file to App folder
Copy files in this directory

![pic5](https://raw.githubusercontent.com/scao7/dbyll/gh-pages/assets/media/androidRes/pic5.PNG)
Create a folder named "jniLibs" and copy the files under the directory

![pic6](https://raw.githubusercontent.com/scao7/dbyll/gh-pages/assets/media/androidRes/pic6.PNG)

#### Use the OpenCV library

```java
ostu.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                OpenCVLoader.initDebug();
                Bitmap originalBitmap = BitmapFactory.decodeResource(getResources(),R.drawable.sample);
                Mat resultMat = new Mat();
                Bitmap ouputBitmap = Bitmap.createBitmap(originalBitmap.getWidth(),originalBitmap.getHeight(),Bitmap.Config.RGB_565);
                Utils.bitmapToMat(originalBitmap,resultMat);
                Imgproc.cvtColor(resultMat,resultMat,Imgproc.COLOR_RGBA2GRAY,0);
                Imgproc.threshold(resultMat,resultMat,0,255, Imgproc.THRESH_OTSU);
                Utils.matToBitmap(resultMat,ouputBitmap);
                imageView.setImageBitmap(ouputBitmap);
            }
        });
```
