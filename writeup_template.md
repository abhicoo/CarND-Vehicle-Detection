##Writeup Template
###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./output_images/car_not_car.png
[image2]: ./output_images/hog_features.png
[image3]: ./output_images/multi_scale_and_positive_windows.png
[image4]: ./output_images/heatmap.png
[image5]: ./output_images/thresholded_heatmap.png
[image6]: ./output_images/final_image.png

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

###Histogram of Oriented Gradients (HOG)

####1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the first code cell of the IPython notebook (or in lines # through # of the file called `some_file.py`).  

I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like. 

I found out that 'YCrCb' color space works best with orientation of 8 and pix_per_cell (8, 8) and cell_per_block (2, 2). I included all the channels to create HOG feature vector. Below is image on how HOG feature looks like for gray scale image.


![alt text][image2]

####2. Explain how you settled on your final choice of HOG parameters.

I found out that 'YCrCb' color space works best with orientation of 8 and pix_per_cell (8, 8) and cell_per_block (2, 2). I included all the channels to create HOG feature vector. Below is image on how HOG feature looks like for gray scale image.

####3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using combination of HOG, Spatial Bin and Color Histogram features. I achieved accuracy of more than 99% on test data.

###Sliding Window Search

####1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I impplemented multiscale sliding window search. The different scale of windows used was  64, 128, 192. The 64 window size was used on smaller region of image to search for cars near horizion. The overlap used is 0.75, 0.7 and 0.6 respectively.

For the final video i used an optimized version of sliding window search by using HOG subsampling so that we have to calucate HOG features only once.
I used window size of 64 but scale of 1.0, 1.5 and 1.75.

![alt text][image3]

####2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Ultimately I searched on three scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  Here are some example images:

![alt text][image3]
---

### Video Implementation

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./output_project_video.mp4)


####2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

### Here is a heatmap image along with original image:

![alt text][image4]

### Here is a threholded heatmap image along with original image:
![alt text][image5]

### Here the resulting bounding boxes are drawn onto the last frame in the series:
![alt text][image6]


---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The most important aspect of the project was to create features to train the classifier. The feature vectors are handcoded one thing which can be done to improve the alogrithm is to use more advance classifier like CNN in which the features are learned by the classifier itself. 
