## Writeup Template
### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

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
[image1]: ./writeup_images/training_data.png
[image2]: ./writeup_images/features.png
[image3]: ./writeup_images/sliding_window.png
[image4]: ./writeup_images/pipeline.png

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in cells 4 – 6 of the IPython notebook.  

I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

Here is an example using the `Gray` color space and HOG parameters of `orientations=8`, `pixels_per_cell=(16, 16)` and `cells_per_block=(1, 1)`:

![alt text][image2]

#### 2. Explain how you settled on your final choice of HOG parameters.

I used gray because the sample images contain vehicles in many different colors. With the amount of features I used I got good results, at the same time the classifier would still be fast.

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a SVM using default parameters. I standardize the features with the `StandardScaler`. I trained the classifier with 80% of the images so I would have 20% for testing which got me to an accuracy of 98.99%.

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I decided to search random window positions at random scales all over the image and came up with this (ok just kidding I didn't actually ;):

I decided on 3 window sizes and 3 areas for these windows. I went for an overlapping of 50% which performs well in terms of speed and results.
I picked the areas so that I would not look for cars outside the lanes. This works well with the video where the car is driving on the very left lane.

![alt text][image3]

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

When I applied the classifier to the windowed regions we get boxes with positive results, many of these overlapped as I got multiple positive results for a single car.
I calculated the average box size by creating a heat map of boxes.

![alt text][image4]
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_video_output.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

This is done by the heat map and the way I set the areas.

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I found the accuracy of the SVM classifier quite impressive, even though this is a controlled setup.
In the sample video there are a few situations where I lose tracking, especially visible when the white car enters the area with the light grey concrete. This could probably be fixed with further image processing.

While I tried to keep my approach lean and perform quickly it is clear that event his somewhat unstable method still takes too much time to run in real-time. When I tried different window sizes or more layers processing time would increase even more.

While the SVM classifier produced good results I'd be curious to see how a neural network would perform.
