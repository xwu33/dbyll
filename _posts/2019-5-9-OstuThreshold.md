---
layout: post
title: Android Development ---- Using Ostu threshold method to get pre-process image
categories: [Android Development]
tags: [Android, Java, Image Processing]
fullview: false
comments: false
shortinfo: This page demostrate the Ostu method in the image processing and Java implementation. Mixed with JavaScript code and MATLAB code.
---

#### Problem Description
I am facing the problem to build an Android App to measure a person's waistline, height and hip data. The first step is to find a correct threshold method to generate a good picture for us to build further algorithms.

#### Theory
All the Theory of Ostu Threshold is from this paper: [Ostu's paper](http://webserver2.tecgraf.puc-rio.br/~mgattass/cg/trbImg/Otsu.pdf). And the reference [JavaScript page](http://hipersayanx.blogspot.com/2016/08/otsu-threshold.html).

#### implementation
First, I tried to implement the basic code for threshold of a certain value for example: 100.
Here is the Java Code in Android Studio
```java
public  static Bitmap converImage(Bitmap original){
        Bitmap finalImage = Bitmap.createBitmap(original.getWidth(), original.getHeight(),original.getConfig());
        int colorPixel;
        int A,R,G,B;
        int width = original.getWidth();
        int height = original.getHeight();
        for(int x = 0; x < width; x++ ){
            for(int y = 0 ; y < height; y++){
                colorPixel = original.getPixel(x,y);
                A = Color.alpha(colorPixel);
                R = Color.red(colorPixel);
                G = Color.green(colorPixel);
                B = Color.blue(colorPixel);

                if(R > 100 ){
                    R = 255;
                }
                else{
                    R = 0;
                }
                G= R;
                B= R;
                finalImage.setPixel(x,y,Color.argb(A,R,G,B));
            }
        }

        return finalImage;
    }
```
#### Modify the fixed threshold into Ostu threshold
continue...
