# SFND 3D Object Tracking

Welcome to the final project of the camera course. By completing all the lessons, you now have a solid understanding of keypoint detectors, descriptors, and methods to match them between successive images. Also, you know how to detect objects in an image using the YOLO deep-learning framework. And finally, you know how to associate regions in a camera image with Lidar points in 3D space. Let's take a look at our program schematic to see what we already have accomplished and what's still missing.

<img src="images/course_code_structure.png" width="779" height="414" />

In this final project, you will implement the missing parts in the schematic. To do this, you will complete four major tasks: 
1. First, you will develop a way to match 3D objects over time by using keypoint correspondences. 
2. Second, you will compute the TTC based on Lidar measurements. 
3. You will then proceed to do the same using the camera, which requires to first associate keypoint matches to regions of interest and then to compute the TTC based on those matches. 
4. And lastly, you will conduct various tests with the framework. Your goal is to identify the most suitable detector/descriptor combination for TTC estimation and also to search for problems that can lead to faulty measurements by the camera or Lidar sensor. In the last course of this Nanodegree, you will learn about the Kalman filter, which is a great way to combine the two independent TTC measurements into an improved version which is much more reliable than a single sensor alone can be. But before we think about such things, let us focus on your final project in the camera course. 

## Dependencies for Running Locally
* cmake >= 2.8
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1 (Linux, Mac), 3.81 (Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* OpenCV >= 4.1
  * This must be compiled from source using the `-D OPENCV_ENABLE_NONFREE=ON` cmake flag for testing the SIFT and SURF detectors.
  * The OpenCV 4.1.0 source code can be found [here](https://github.com/opencv/opencv/tree/4.1.0)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory in the top level project directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./3D_object_tracking`.

## For Rubric Fulfillment

## FP.1 Match 3D Objects

**CRITERIA**
Implement the method "matchBoundingBoxes", which takes as input both the previous and the current data frames and provides as output the ids of the matched regions of interest (i.e. the boxID property). Matches must be the ones with the highest number of keypoint correspondences.

**Solution**
We first loop over all the matches to find the unique matched candidate pair. Store the keypoint information from previous and current frame by matching using these attributes 'queryIdx' and 'trainIdx'.Look for this matched points in the previous frame roi and current frame roi. In a 2D array store the frequency of box pairs. Once the box pairs are collected , select the index with max score. This has been implemented in function `matchBoundingBoxes` and can be found in the `camFusion_Student.cpp` file.

## FP.2 Compute Lidar-based TTC

**CRITERIA**
Compute the time-to-collision in second for all matched 3D objects using only Lidar measurements from the matched bounding boxes between current and previous frame.

**Solution**
Used the same model as discussed during the lesson.This has been implemented in function `computeTTCLidar` and can be found in the `camFusion_Student.cpp` file.

## FP.3 Associate Keypoint Correspondences with Bounding Boxes

**CRITERIA**
Prepare the TTC computation based on camera measurements by associating keypoint correspondences to the bounding boxes which enclose them. All matches which satisfy this condition must be added to a vector in the respective bounding box.

**Solution**
First calculate mean point match distance in the bbox.Then associate the given keypoints of bounding box with the corresponding matches.This has been implemented in function `clusterKptMatchesWithROI` and can be found in the `camFusion_Student.cpp` file.

## FP.4 Compute Camera-based TTC

**CRITERIA**
Compute the time-to-collision in second for all matched 3D objects using only keypoint correspondences from the matched bounding boxes between current and previous frame.

**Solution**
Used the same model as discussed during the lesson.This has been implemented in function `computeTTCCamera` and can be found in the `camFusion_Student.cpp` file.

## FP.5 Performance Evaluation 1

**CRITERIA**
Find examples where the TTC estimate of the Lidar sensor does not seem plausible. Describe your observations and provide a sound argumentation why you think this happened.

**Solution**
The example images are stored in the folder results/images(img1_TTClidar_new,img2_TTClidar_new,img3_TTClidar_new). In this folder there are 3 images of continuous frames.TTC of lidar increases from 12s to 31s and then drops to 14s.A small change of 0.03 m brings out a big fluctuation. There is ared signal infront and ego car moves slowly.

## FP.5 Performance Evaluation 2

**CRITERIA**
Run several detector / descriptor combinations and look at the differences in TTC estimation. Find out which methods perform best and also include several examples where camera-based TTC estimation is way off. As with Lidar, describe your observations again and also look into potential reasons.

**Solution**
The results are avilable in results/PerformanceEvaluation_3D.xlsx. Based on the best combinations selected for the Mid term project tried to compute TTC camera estimate.

There are matched points in the ground and other cars which violates our assumption . The camera TTC is very unstable compared to lidar TTC. Images stored in results/camera_roadmatching.png
