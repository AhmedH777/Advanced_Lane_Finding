## Advanced Lane Finding Project

The Project
---

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

[image1]: ./examples/undistort_output.png "Undistorted"
[image2]: ./examples/undistort.png "Undistorted Road"
[image3]: ./examples/filters1.png "filters1"
[image4]: ./examples/filters2.png "filters2"
[image5]: ./examples/filters3.png "filters3"
[image6]: ./examples/warped.png "warped"
[image7]: ./examples/corners.png "corners"
[image8]: ./examples/lane_lines.png "lane_lines"
[image9]: ./examples/raduis_of_curv.png "raduis_of_curv"
[image10]: ./examples/lane_data.png "lane_data"
[video1]: ./test_videos_output/project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.
 
I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.

![alt text][image7]

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

Its clear that the objects in the undistorted image on the right are further away than the distorted image captured by the camera on the left.

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image.For each image i applied multiple filters as shown in the image below to figure out the best combinations that segment the lane lines with the least amount of noise.

The result combined filter is the found in **section 3 Gradient and Color Thresholding**

The final combined filter is the function *combined_filter()* which is the combination of multiple filters as follows:

**[ [ ( X-gradient AND L-channel ) OR ( Magnitude-gradient AND Directional-gradient ) ] AND S-channel ] OR R-channel ]**


![alt text][image3]

![alt text][image4]

![alt text][image5]


#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warp_img()`, which appears in section 2 Perspective Transform.  The `warp_img()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:


	src = np.float32([(590,450),
                  (710,450), 
                  (280,680), 
                  (1100,680)])

    dst = np.float32([(450,0),
                      (w-450,0),
                      (450,h),
                      (w-450,h)])
```
```

![alt text][image6]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

I used the sliding window technique (section 4 Sliding Window) which starts by using the histogram of the binary image then detects the peaks of the binary image and the highest two peaks represents the lane start position which shall be the start of the sliding window. 

Then after calculating all the sliding windows we can fit a polynomial for each lane line. The implementation of this method can be found in the function sliding_window_line_fitting() 

Here is a result the Sliding window on a test image:

![alt text][image8]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in the section 5 Raduis of Curvature which is calculated in the function *calc_curv_rad_and_center_dist()* 

![alt text][image9]

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I did this in the section 6 Draw Lanes and Data which is calculated in the functions *draw_lanes()*  and *draw_data()*


![alt text][image10]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video.

Here's a [link to my video result](./test_videos_output/project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The main problem was in getting the correct combination of filters to have the least noise and filter out the lane lines.

I think to make the code more robust it should be tried on different videos in different light conditions to get the perfect combination in different scenes and situations.
"# Advanced_Lane_Finding" 
