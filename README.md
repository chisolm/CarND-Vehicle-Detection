# Vehicle Detection
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

---

**Vehicle Detection Project**
---

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

---

[//]: # (Image References)

[image1]: ./output_images/find_cars.jpg "Single run find cars"
[image2]: ./output_images/heat_map.jpg "Heat Map"
[image3]: ./output_images/heat_map_mult.jpg "Heat Map Multiple Call"

[video1]: ./project_video_output.mp4 "Video"


## [Rubric Points](https://review.udacity.com/#!/rubrics/513/view)

Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. You're reading it!


### Histogram of Oriented Gradients (HOG)

#### Choices of Parameters

Runtime performance drove several of my choices of parameters.  pix_per_cell of 16 gave significantly better run
time performance than 8 with a limited but noticeable identification penalty.   In the video we tested, I noticed
no identification penalty for using cell_per_block of 1.  I could easily see this being a poor choice in 
changing lighting conditions or with shadows.

I tried orient of 9 and 12.  Noticed no significant change and defaulted to the value used in class of 9.

I did not try any changes to spatial_size or hist_bins.

#### Explain How Classifier Was Trained

The classifier that was used was a LinearSVC().  I split the data using the train_test_split(), reserving 
20% for test.  The random state start assures that this is shuffled from training run to training run.

I had a focus on reducing the feature vector length which did reduce the training time significantly.  The
final set of parameters generally trained in 3.5s

### Sliding Window Search

#### Implementation


Again I had an approach oriented to performance.  I used the hog sub-sampling window search from lesson
35.  

https://classroom.udacity.com/nanodegrees/nd013/parts/fbf77062-5703-404e-b60c-95b78b2f3f9e/modules/2b62a1c3-e151-4a0e-b6b6-e424fa46ceab/lessons/fd66c083-4ccb-4fe3-bda1-c29db76f50a0/concepts/c3e815c7-1794-4854-8842-5d7b96276642

I added the heat map and label methods from other lessons to remove false positives and remove duplicates 
respectively.  I implementd a small class to track and management the heatmap values from frame to frame.

![alt text][image1]

#### Test of pipeline

The basic operation of find_cars can be seen above.

The addition of a head map is shown with an image with overlapping boxes contributing to the
heat map:

![alt text][image2]

Multiple calls to process image show the accummulation of data in the heat map.

![alt text][image3]

some discussion on how to improve reliability classifier - could be fewer false positives and more reliable car detections (this could be things like choice of feature vector, thresholding the decision function, hard negative mining etc.)

#### Video Implementation


Here's a [link to my video result.](./project_video_output.mp4)

I faced enormous problems dealing with poor choices and inconsistent choices in image format as
a result of using code borrowed from multiple lessons and not going through and using a consistent
model for image passing.  It is mostly cleaned up, but was a huge time waste.  And I knew about that
risk prior to implementation.

My pipeline looks good, the heatmap filtering performs well and is easily adjusted to different 
conditions. Right now it is tuned for low levels of detected bounding boxes.  If the classifier is
corrected it will likely need to change.

My classifier is my number one problem by far and it needs additional work.  It does classify cars
correctly most of the time, but it does not do it all the time where it very resonably should.  It
would be very useful to generate a new set of training data based on the video data to compare the training
of the model with the data set I do use.

