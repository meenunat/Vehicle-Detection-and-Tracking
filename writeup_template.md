# Project 5:  **Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier.
* A color transformation along with spatial binning and histograms of colors were applied to the HOG feature vector. 
* The features were normalized and the selection for training and testing data was randomized.
* A sliding-window technique was implemeneted and the trained classifier was used to search for vehicles in images.
* The pipeline was used on a video stream and a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles was created.
* Estimate a bounding box for vehicles detected.

## Histogram of Oriented Gradients (HOG)

### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the second code cell of the IPython notebook -`vehicle_detection.ipynb`  

I started by reading in all the `vehicle` and `non-vehicle` images into cars and not_cars list.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes.

### 2. Explain how you settled on your final choice of HOG parameters.

I tried various various color space in order (`'RGB', 'HSV', 'LUV', 'HLS', 'YUV', 'YCrCb'`) and HOG parameters of `orientations=9`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`.
 
#### HOG image for RGB Color Space 
![](./output_images/rgb_hog_image.png?raw=true "RGB")

#### HOG image for HSV Color Space
![](./output_images/hsv_hog_image.png?raw=true "HSV")

#### HOG image for LUV Color Space
![](./output_images/luv_hog_image.png?raw=true "LUV")


#### HOG image for HLS Color Space
![](./output_images/hls_hog_image.png?raw=true "HLS")

#### HOG image for YUV Color Space
![](./output_images/yuv_hog_image.png?raw=true "YUV")

#### HOG image for yCrCb Color Space
![](./output_images/yCrCb_hog_image.png?raw=true "YCrCb")

Among the various color space, YCrCb appears to provide the best result. 

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

The code for this step is contained in the fourth code cell of the IPython notebook. 

The data was shuffled and split into validation, test data. The data was normalized using sklearn package `StandardScaler()`. A Linear SVC classifier was used combined with spatial binning, color histogram and hog classification.  With this the test accuracy is approx 0.9875.

### Sliding Window Search and Heat Map

With the Test accuracy as 0.9875, there is still false positives in the detection. Also there is some regions that overlaps with other windows.

This is addressed by defining an area of interest where the cars will be present and applying threshold to the heat map. 

The code for this step is contained in the fourth and fifth code cell of the IPython notebook.Few test images output after applying the window search and threshold to the heat map.

#### Test image Output with bounding boxes and Heat Map
![](./output_images/output_single_image.png?raw=true "Output")

### Video Implementation

Here's a [link to my video result](./project_output.mp4)

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The algorithm false identifies the trees unless the area of interest is defined. But the area of interest defined using test images may change for project_video.mp4.

One other limitation is that the algorithm identifies the vehicle on the other side of the road due to wider range of area of interest. 

Also, the size of the box keeps changing during the video process that needs to be improved.

The algorithm is trained with images that restrict it to identify the vehicle only after the entire vehicle is in the frame which needs to be addressed.

### References
1. [Udacity Self-Driving Car Class Materials](https://classroom.udacity.com/nanodegrees/nd013/parts/fbf77062-5703-404e-b60c-95b78b2f3f9e)
2. [Self-Driving Car Project Q&A](https://www.youtube.com/watch?v=P2zwrTM8ueA&feature=youtu.be&utm_medium=email&utm_campaign=2017-05-24_carnd_projectwalkthroughs&utm_source=blueshift&utm_content=2017-05-24_carnd_projectwalkthroughs&bsft_eid=809c46b1-7b0f-4960-9cc1-459c102110d5&bsft_clkid=d5c545c7-51a9-4ad1-b183-8df98e93bfce&bsft_uid=6462eace-49a4-468f-a7d8-d25690fd3bfa&bsft_mid=b9348cc6-ddd2-408e-9441-aa9a1302ad86)