
**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./output_images/undistort.png "Undistorted"
[image2]: ./output_images/5.png "Perspective Transformed"
[image3]: ./output_images/color_gradient.png "Binary Example"
[image4]: ./output_images/draw.png "Projected path"
[image5]: ./output_images/chessboard.png "Chessboard"
[image6]: ./output_images/fit.png "Fit Lines"
[video1]: ./project_video_output.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation in [Advanced_Lane_Lines](Advanced_Lane_Lines.ipynb).  

---

### Camera Calibration

The code for this step is contained in the IPython notebook located in  [Camera_Calibration](Camera_Calibration.ipynb).

I assume the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image. `objpoints` will contain the object points.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection in a test image using `cv2.findChessboardCorners`.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image5]

### Pipeline (single images)

#### 1. Calibrate and Undistort Images

As stated above, the undistortion is done in [Camera_Calibration](Camera_Calibration.ipynb). Here is an example of the result by applying it to the test images using the saved matrix:
![alt text][image1]

#### 2. Color and Gradient Thresholds

I used a combination of color and gradient thresholds to generate a binary image in [Color_Gradient](Color_Gradient.ipynb). I defined a function `color_gradient()` in [Advanced_Lane_Lines](Advanced_Lane_Lines.ipynb) as part of the pipeline. Here is an example of my output for this step.

![alt text][image3]

#### 3. Perspective Transform
I hardcoded the perspective transform and calculated the transformation matrix in cell 5 of the notebook [Perspective_Transform](Perspective_Transform.ipynb).
The code for my perspective transform on the test images includes a function called `warp()`, which appears in lines 1 through 8 in the notebook [Advanced_Lane_Lines](Advanced_Lane_Lines.ipynb)

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image. Here is an example.

![alt text][image2]

#### 4. Polynomial fit of the left and right lines

Using the sliding window technique, I find the points in each window for the left and right lines separately. I then fitted a line through the points using numpy.polyfit(). Here is an example on the test images:

![alt text][image6]

#### 5. Curvature Radius and Car Position Calculation 

I did this in cells 18 and 19 in my code in [Advanced_Lane_Lines](Advanced_Lane_Lines.ipynb). I first calculated the pixel radius and then convert it to real life distance based on `ym_per_pix`, which is the meter per pixel value.

To find the position of the vehicle with respect to the lane, I compared its position, which is assumed to be the middle of the frame to the midpoint between fitted lines.

#### 6. Lane Projection

I plotted the lane back down onto the road image.

I implemented this step in cells 20 and 21 in the notebook [Advanced_Lane_Lines](Advanced_Lane_Lines.ipynb).  Here is an example of my result on a test image:

![alt text][image4]

---

### Video Output

I applied this process to the project video. Here's a [link to my video result](./project_video_output.mp4)

---

### Discussion

My approach didn't do well with the challenge video. It could be because of the road having multiple shades on the left. The right white line seems fine. Other gradient approaches could be used to improve line detection.
