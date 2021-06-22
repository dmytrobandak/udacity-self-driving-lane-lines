# **Finding Lane Lines on the Road**
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

## Overview

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm. In this project we detect lane lines in images using Python and OpenCV.

[//]: # (Image References)

[image1]: ./writeup_images/1.png
[image2]: ./writeup_images/2.png
[image3]: ./writeup_images/3.png
[image4]: ./writeup_images/4.png
[image5]: ./writeup_images/5.png

## Pipeline.

My pipeline consisted of the following steps:
- Convolve the image with a Gaussian kernel (in "process_image" of Helper Functions).  This step is intended to remove noise from images.
- Convert the images to grayscale (in "process_image" of Helper Functions). This step is intended to prep the image for Canny algorithm in the next step.

![alt text][image1]
- Detect the edges in the grayscale image using the Canny algorithm (in "process_image" of Helper Functions).

![alt text][image2]
- Apply a mask to the image of edges in order to only keep the image segment with lane lines (in "region_of_interest" of Helper Functions).

![alt text][image3]
- Extract the line features (white and yellow, no distinction by color) with HoughLinesP from OpenCV that utilizes Hough transform (in "process_image" of Helper Functions).
- Calculate the slopes for each line feature using its end points. Then define two windows of slopes that are appropriate for left and right lane line. Assign each line feature to left lane line, right lane line, or neither.
- Use the end points of line features of the left and right group to create linear fits that serve as our estimate of the lane lines.

![alt text][image4]
- Superimpose the original image with the lane lines image using addWeighted from OpenCV.

![alt text][image5]

## Potential shortcomings with the current pipeline.

- There may be marks on the road that are not lane lines. Possible sources include imperfections of concrete and tire marks. Such marks clutter the image and create noise. In the case when these marks are dominant in the picture, identifying the lane lines may become impossible using the current pipeline.

- Lane line may be worn out, faint, or absent completely. For such a segment of the road identifying the position of lane lines with current pipeline is impossible.

## Possible improvements to the pipeline.

- One possible strategy to improve upon the current pipeline is to introduce memory. By the former I mean a mechanism that incorporates recent history of lane line position into current lane line identification. One would need to take into account how fast lane lines can change their position given frames per second of the camera and typical speeds of the vehicle.

- Another strategy is to use the information about color. Given that lane lines are either yellow or white and do not change color, one can leverage this knowledge by applying a color mask to remove noise such as tire marks (black).
