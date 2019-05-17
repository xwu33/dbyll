---
layout: post
title: Android Development ---- Import OpenCV
categories: [Android Development]
tags: [Android, Java, Image Processing]
fullview: false
comments: false
shortinfo: From previews blog, I have the method to do the Ostu thresholding individually. This blog is going to talk about an easy way to do Ostu thresholding which is rely on the OpenCV library. I will demonstrate how to import the OpenCV library in Android and how to apply it to image Processing.
---

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
![pic3](https://raw.githubusercontent.com/scao7/dbyll/gh-pages/assets/media/androidRes/pic2.PNG)

![pic4](https://raw.githubusercontent.com/scao7/dbyll/gh-pages/assets/media/androidRes/pic2.PNG)

#### Add dependency

```
File --> Project Structure --> App --> Add dependency --> OpenCV
```
#### Copy library file to App folder
