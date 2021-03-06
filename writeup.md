## Project: Search and Sample Return
### Writeup Template: You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---


**The goals / steps of this project are the following:**  

**Training / Calibration**  

* Download the simulator and take data in "Training Mode"
* Test out the functions in the Jupyter Notebook provided
* Add functions to detect obstacles and samples of interest (golden rocks)
* Fill in the `process_image()` function with the appropriate image processing steps (perspective transform, color threshold etc.) to get from raw images to a map.  The `output_image` you create in this step should demonstrate that your mapping pipeline works.
* Use `moviepy` to process the images in your saved dataset with the `process_image()` function.  Include the video you produce as part of your submission.

**Autonomous Navigation / Mapping**

* Fill in the `perception_step()` function within the `perception.py` script with the appropriate image processing functions to create a map and update `Rover()` data (similar to what you did with `process_image()` in the notebook). 
* Fill in the `decision_step()` function within the `decision.py` script with conditional statements that take into consideration the outputs of the `perception_step()` in deciding how to issue throttle, brake and steering commands. 
* Iterate on your perception and decision function until your rover does a reasonable (need to define metric) job of navigating and mapping.  

[//]: # (Image References)

[image1]: ./figures/searched.png
[image2]: ./figures/collected.png
[image3]: ./figures/threshed.png 

## [Rubric](https://review.udacity.com/#!/rubrics/916/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it!

### Notebook Analysis
#### 1. Run the functions provided in the notebook on test images (first with the test data provided, next on data you have recorded). Add/modify functions to allow for color selection of obstacles and rock samples.
The functions are tested on both the provided data and the collected data (under my_dataset), and the function `search_rocks()` is added to allow for color selection of rock samples, such an example shown below.

![alt text][image1]
#### 2. Populate the `process_image()` function with the appropriate analysis steps to map pixels identifying navigable terrain, obstacles and rock samples into a worldmap.  Run `process_image()` on your test data using the `moviepy` functions provided to create video output of your result.
In the `process_image()` function, perspective transform is applied on the input image through the defined source and destination points, then the `color_thresh()` and `search_rocks()` functions are used to identify navigable terrain/obstacles/rock samples, example shown below.  The worldmap is updated to include the original image in the upper left hand corner, the warped image in the upper right hand corner, and the worldmap is overlayed with the ground truth map.  Then use the moviepy library to create a video based on the collected data (my_mapping.mp4 under output).

![alt text][image2]
### Autonomous Navigation and Mapping

#### 1. Fill in the `perception_step()` (at the bottom of the `perception.py` script) and `decision_step()` (in `decision.py`) functions in the autonomous mapping scripts and an explanation is provided in the writeup of how and why these functions were modified as they were.
In the `perception_step()` function, the pre-defined functions are applied in succession and the Rover state is updated accordingly: On left side of screen, Rover.vision_image is embedded using color-thresholded binary images for navigable terrain, obstacles and rock samples; on right side of screen, Rover.worldmap is equipped with the detected information.

The basic project template for the `decision_step()` function is used here.  It depends on the perception result to decide how to switch between the forward and stop mode: If mode is forward and navigable terrain looks good, the throttle value is set according to whether the velocity is above or below Rover.max_vel, otherwise change to stop mode; If mode is stop and still moving, keep braking, otherwise change to forward mode or induce 4-wheel turning according to whether there's sufficient navigable terrain in front.  In fact, this project can incorporate Behavioral Cloning method: By collecting more training data, some deep learning algorithm, for instance, CNN, can be trained to learn how to set throttle, brake and steer based on the vision data.
#### 2. Launching in autonomous mode your rover can navigate and map autonomously.  Explain your results and how you might improve them in your writeup.  

**Note: running the simulator with different choices of resolution and graphics quality may produce different results, particularly on different machines!  Make a note of your simulator settings (resolution and graphics quality set on launch) and frames per second (FPS output to terminal by `drive_rover.py`) in your writeup when you submit the project so your reviewer can reproduce your results.**

Simulator is set with 1024 x 768 resolution and good graphics quality.  After launching in autonomous mode, the rover finds rock samples pretty soon.  To improve this project, the `decision_step()` function should be modified to include the procedure to bring collected samples to the target.
