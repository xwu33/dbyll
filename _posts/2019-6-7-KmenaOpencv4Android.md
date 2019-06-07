---
layout: post
title: Android Development ---- K-mean
categories: [Android Development]
tags: [Android, Java, Image Processing, OpenCV]
fullview: false
comments: false
shortinfo: This blog is going to describe the K-mean clustering method for image processing using OpenCV.
---
#### Understand K-mean Algorithms
K-mean clustering method originally from signal processing, this method is the popular for cluster analysis in data mining. The goal of this algorithms is to partition n observations into K clusters in which each observation belongs to the cluster with the nearest mean.

#### What I use K-mean for
In my projects, I need to analysis human body shape and get some health information. I need to distinguish the cloth and human body. Therefore, I need use k-mean to cluster the colors.

#### Code for k-mean
```java
public void k_Mean(){
        Mat rgba = new Mat();
        Mat mHSV = new Mat();

        Bitmap bitmap = BitmapFactory.decodeResource(getResources(),images[current_image]);
        Bitmap outputBitmap = Bitmap.createBitmap(bitmap.getWidth(),bitmap.getHeight(), Bitmap.Config.RGB_565);
        Utils.bitmapToMat(bitmap,rgba);

        //must convert to 3 channel image
        Imgproc.cvtColor(rgba, mHSV, Imgproc.COLOR_RGBA2RGB,3);
        Imgproc.cvtColor(rgba, mHSV, Imgproc.COLOR_RGB2HSV,3);
        Mat clusters = cluster(mHSV, 3).get(0);
        Utils.matToBitmap(clusters,outputBitmap);

        imageView.setImageBitmap(outputBitmap);
    }


    public static List<Mat> cluster(Mat cutout, int k) {
        Mat samples = cutout.reshape(1, cutout.cols() * cutout.rows());
        Mat samples32f = new Mat();
        samples.convertTo(samples32f, CvType.CV_32F, 1.0 / 255.0);

        Mat labels = new Mat();
        //criteria means the maximum loop
        TermCriteria criteria = new TermCriteria(TermCriteria.COUNT, 20, 1);
        Mat centers = new Mat();
        Core.kmeans(samples32f, k, labels, criteria, 1, Core.KMEANS_PP_CENTERS, centers);

        return showClusters(cutout, labels, centers);
    }

    private static List<Mat> showClusters (Mat cutout, Mat labels, Mat centers) {
        centers.convertTo(centers, CvType.CV_8UC1, 255.0);
        centers.reshape(3);

        System.out.println(labels + "labels");

        List<Mat> clusters = new ArrayList<Mat>();
        for(int i = 0; i < centers.rows(); i++) {
            clusters.add(Mat.zeros(cutout.size(), cutout.type()));
        }

        Map<Integer, Integer> counts = new HashMap<Integer, Integer>();
        for(int i = 0; i < centers.rows(); i++) counts.put(i, 0);

        int rows = 0;
        for(int y = 0; y < cutout.rows(); y++) {
            for(int x = 0; x < cutout.cols(); x++) {
                int label = (int)labels.get(rows, 0)[0];
                int r = (int)centers.get(label, 2)[0];
                int g = (int)centers.get(label, 1)[0];
                int b = (int)centers.get(label, 0)[0];
                counts.put(label, counts.get(label) + 1);
                clusters.get(label).put(y, x, b, g, r);
                rows++;
            }
        }

        System.out.println(counts);
        return clusters;
    }
```

#### limitation
These code for k-mean takes a long time to run on the android phone. Probably due to the showClusters function take a two for loop to get a clusters images. I am looking for good way to make the method faster. Once I got a way, I will continue to write in this post.
