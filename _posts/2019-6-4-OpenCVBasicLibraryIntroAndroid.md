---
layout: post
title: Android Development ---- Basic image processing tool
categories: [Android Development]
tags: [Android, Java, Image Processing, OpenCV]
fullview: false
comments: false
shortinfo: This blog will list the solution of easy OpenCV library of image processing and examples.
---
#### Image Gradient
```java
private void imageGradient(){
     Mat grayMat = new Mat();
     Mat sobel = new Mat();
     Mat grad_x = new Mat();
     Mat abs_grad_x = new Mat();
     Mat grad_y = new Mat();
     Mat abs_grad_y = new Mat();
     Bitmap bitmap = BitmapFactory.decodeResource(getResources(),images[current_image]);
     Mat originalMat = new Mat();
     Utils.bitmapToMat(bitmap,originalMat);
     Imgproc.cvtColor(originalMat, grayMat,Imgproc.COLOR_RGBA2GRAY);
     Imgproc.Sobel(grayMat,grad_x,CvType.CV_16S,1,0,3,1,0);
     Imgproc.Sobel(grayMat,grad_y,CvType.CV_16S,0,1,3,1,0);
     Core.convertScaleAbs(grad_x,abs_grad_x);
     Core.convertScaleAbs(grad_y,abs_grad_y);
     Core.addWeighted(abs_grad_x,0.5,abs_grad_y,0.5,1,sobel);
     Bitmap currentBitmap = Bitmap.createBitmap(bitmap.getWidth(),bitmap.getHeight(),Bitmap.Config.RGB_565);
     Utils.matToBitmap(sobel,currentBitmap);
     imageView.setImageBitmap(currentBitmap);
 }
```
#### Canny Contours
```java
private void cannyContours(){
     Bitmap originalbitmap = BitmapFactory.decodeResource(getResources(),images[current_image]);
     Mat originalMat = new Mat();
     Utils.bitmapToMat(originalbitmap,originalMat);

     Mat grayMat = new Mat();
     Mat cannyEdges = new Mat();
     Mat hierarchy = new Mat();

     List<MatOfPoint> contourList = new ArrayList<MatOfPoint>();

     Imgproc.cvtColor(originalMat,grayMat,Imgproc.COLOR_RGBA2GRAY);
     Imgproc.Canny(grayMat,cannyEdges,10,100);
     Imgproc.findContours(cannyEdges,contourList,hierarchy,Imgproc.RETR_LIST,Imgproc.CHAIN_APPROX_SIMPLE);

     Mat contours = new Mat();
     contours.create(cannyEdges.rows(),cannyEdges.cols(),CvType.CV_8UC3);
     Random r = new Random();

     for (int i = 0; i< contourList.size();i++){
         Imgproc.drawContours(contours,contourList,i,new Scalar(r.nextInt(255),r.nextInt(255),r.nextInt(255)),-1);
     }

     Bitmap currentBitmap = Bitmap.createBitmap(originalbitmap.getWidth(),originalbitmap.getHeight(),Bitmap.Config.RGB_565);
     Utils.matToBitmap(contours,currentBitmap);
     imageView.setImageBitmap(currentBitmap);
 }
```
#### Blur
```java
private void blur(){
        Bitmap originalbitmap = BitmapFactory.decodeResource(getResources(),images[current_image]);
        Mat mat = new Mat();
        Utils.bitmapToMat(originalbitmap,mat);
        Imgproc.blur(mat,mat,new Size(10,10));
        Utils.matToBitmap(mat,originalbitmap);
        imageView.setImageBitmap(originalbitmap);
    }
```
#### Median Blur
```java
private void medianBlur(){
    Bitmap originalbitmap = BitmapFactory.decodeResource(getResources(),images[current_image]);
    Mat mat = new Mat();
    Utils.bitmapToMat(originalbitmap,mat);
    Imgproc.medianBlur(mat,mat,3);
    Utils.matToBitmap(mat,originalbitmap);
    imageView.setImageBitmap(originalbitmap);
}
```

#### Dilate
```java
private void dilate(){
    Bitmap originalbitmap = BitmapFactory.decodeResource(getResources(),images[current_image]);
    Mat mat = new Mat();
    Utils.bitmapToMat(originalbitmap,mat);
    Imgproc.dilate(mat,mat,Imgproc.getStructuringElement(Imgproc.MORPH_RECT,new Size(3,3)));
    Utils.matToBitmap(mat,originalbitmap);
    imageView.setImageBitmap(originalbitmap);
}
```
to be continue 
