---
layout: post
title: Android Development ---- Using Ostu threshold method to measure height
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
This is the method that applied the Ostu threshold for the colored image

```java
public static Bitmap ostuConvert(Bitmap original){
      Bitmap BWimg = Bitmap.createBitmap(original.getWidth(), original.getHeight(), original.getConfig());
      int width = original.getWidth();
      int height = original.getHeight();
      int A, R, G, B, colorPixel;

      double Wcv = 0, th = 0;
      int[] tPXL = new int[256];
      int[][] pxl = new int[width][height];
      double Bw, Bm, Bv, Fw, Fm, Fv;
      int np, ImgPix = 0, fth = 0;

      // pixel check for histogram //
      for (int x = 0; x < width; x++) {
          for (int y = 0; y < height; y++) {

              colorPixel = original.getPixel(x, y);

              A = Color.alpha(colorPixel);
              R = Color.red(colorPixel);
              G = Color.green(colorPixel);
              B = Color.blue(colorPixel);

              int gray = (int) ( (0.2126 * R) + (0.7152 * G) + (0.0722 * B) ); // (int) ( (0.299 * R) + (0.587 * G) + (0.114 * B) );
              pxl[x][y] = gray;
              tPXL[gray] = tPXL[gray] + 1;
              ImgPix = ImgPix + 1;
          }
      }
      // ----- histo-variance ----- //
      for (int t = 0; t < 256; t++){
          Bw = 0; Bm = 0; Bv = 0;
          Fw = 0; Fm = 0; Fv = 0;
          np = 0;

          if (t == 0){ // all white/foreground as t0 ----- //
              Fw = 1;

              for (int d = 0; d < 256; d++) { //mean
                  Fm = Fm + (d * tPXL[d]);
              }
              Fm = Fm / ImgPix;

              for (int e = 0; e < 256; e++) { //variance
                  Fv = Fv + (Math.pow((e - Fm), 2) * tPXL[e]);
              }
              Fv = Fv / ImgPix;

          }
          else { // main thresholding
              for (int d = 0; d < (t-1); d++){ // BG weight & mean + BG pixel
                  Bw = Bw + tPXL[d];
                  Bm = Bm + (d * tPXL[d]);
                  np = np + tPXL[d];
              }
              Bw = Bw / ImgPix;
              Bm = Bm / np;

              for (int e = 0; e < (t-1); e++) { //BG variance
                  Bv = Bv + (Math.pow((e - Bm), 2) * tPXL[e]);
              }
              Bv = Bv / np;

              for (int j = t; j < 256; j++) { // FG weight & mean + BG pixel
                  Fw = Fw + tPXL[j];
                  Fm = Fm + (j * tPXL[j]);
                  np = ImgPix - np;
              }
              Fw = Fw / ImgPix;
              Fm = Fm / np;

              for (int k = t; k < 256; k++) { //FG variance
                  Fv = Fv + (Math.pow((k - Fm), 2) * tPXL[k]);
              }
              Fv = Fv / np;

          }

          // within class variance
          Wcv = (Bw * Bv) + (Fw * Fv);

          if (t == 0){
              th = Wcv;
          }
          else if (Wcv < th){
              th = Wcv;
              fth = t;
          }
      }

      // set binarize pixel
      for (int x = 0; x < width; x++) {
          for (int y = 0; y < height; y++) {

              int fnpx = pxl[x][y];
              colorPixel = original.getPixel(x, y);

              A = Color.alpha(colorPixel);

              if (fnpx > fth) { //R > fth
                  fnpx = 255;
                  BWimg.setPixel(x, y, Color.argb(A, fnpx, fnpx, fnpx));
              }
              else {
                  fnpx = 0;
                  BWimg.setPixel(x, y, Color.argb(A, fnpx, fnpx, fnpx));
              }
          }
      }
      return BWimg;
  }
```

#### Height Calculation
The Height Calculation is just count the pixel in the height. So we can apply the method above and change a little. In the last for loop: "//set binarize pixel" we modify the method as following:
```java
    float top =  Float.POSITIVE_INFINITY;
    float bottom =  Float.NEGATIVE_INFINITY;
    // set binarize pixel
    for (int x = 0; x < width; x++) {
       for (int y = 0; y < height; y++) {

           int fnpx = pxl[x][y];
           colorPixel = original.getPixel(x, y);

           A = Color.alpha(colorPixel);

           if (fnpx > fth) { //R > fth
               fnpx = 255;
           }
           else {
               fnpx = 0;
               if(top > y ){
                   top = y;
               }

               if(bottom < y){
                   bottom = y;
               }

           }
       }
    }
    float[] pointsY = new float[]{ top * scaleFactor ,bottom * scaleFactor};

    return pointsY;
```
I defined the two float variable initialed as POSITIVE_INFINITY and NEGATIVE_INFINITY. When fnpx <= fth begin to calculate the value. Find the minimun role and the maximum role.
I returned the float arrary represent two height value.
I should post the picture of processing but because of the privacy issue I won't post it until this research period end.
#### Draw line and display pixel on the image view
After we have the value of top and bottom. I need to draw a red line on the image and display the pixel value between these two.

```java
public static void drawHeight(ImageView imgView, Bitmap imageBitmap){
        Bitmap bitmap = Bitmap.createBitmap(imgView.getWidth(), imgView.getHeight(), Bitmap.Config.ARGB_8888);
        float scaleFactor = (float) bitmap.getHeight() / (float) imageBitmap.getHeight();
        float[] points =customizeMethod.ostuCalculateHeight(imageBitmap, scaleFactor);

        Canvas canvas = new Canvas(bitmap);
        imgView.draw(canvas);

        Paint paint = new Paint();
        paint.setColor(Color.RED);
        paint.setStrokeWidth(10);

        float middle = imgView.getWidth() / 2; //x value of the image

        canvas.drawLine(middle, points[0], middle, points[1], paint);
        String pixelInfo = (int)(points[1] - points[0]) + "pixels";

        paint.setColor(Color.GREEN);
        paint.setTextSize(40);
        canvas.drawText(pixelInfo,middle, (points[0] + points[1])/2, paint);
        //draw line on image
        imgView.setImageBitmap(bitmap);
    }
```
I create this method to draw the height. There are two variable we need pass in: imgView and imageBitmap. ImageView refers the view you want to put picture, imageBitmap is the original image in Bitmap data structure.

#### Problems remain to solve
##### <span style="color:blue">Runtime</span>
So far, my app works for measure the height of a person in most circumstance. But the runtime is very slow because I put all the calculation in one thread. There are two option to make the calculation fast. The first one is parallel computing. The second is to use the alternative OpenCV solution, it might faster than my solution. So next post I will focus on how to do parallel computing on Android software and how to load OpenCV library and apply the method.

##### <span style="color:blue">Accurate of measurement</span>
For my application, there still some in accurate value because of the limitation of Ostu threshold method. If a person waring dark cloth and has white hair the output will be inaccurate. Therefore, to increase the accurate of the measurement. I need implement another image processing method called Image gradient. I will introduce the implementation in next blog post.
