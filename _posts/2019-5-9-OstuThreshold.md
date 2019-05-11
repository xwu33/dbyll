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
continue...
