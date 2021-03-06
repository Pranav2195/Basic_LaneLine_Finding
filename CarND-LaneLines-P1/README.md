# **Finding Lane Lines on the Road** 


### Overview:

---
This project utilises basic techniques of computer vision to identify lane lines in a road image. The pipeline developed fro this purpose will be first tested on images, and then on multiple video streams(which is nothing but a series of images) 

---
---

### Project Goal:
---
This project is the first attempt to learn about the techniques involved in Self Driving Cars, starting with a simple Lane finding task.
The steps involved in this attempt ar as follows:
* develop and test an initial pipeline that can identify lane lines in road images
* Identify shortcomings and improve the pipeline within the current scope of my knowledge.
* Identify further areas of improvement that will be the learning objectives for future tasks (Reflection).



[//]: # (Image References)

[gray]: ./test_image_output/gray.png "Grayscale"
[edges]: ./test_image_output/edges.png "edges"
[RoI]: ./test_image_output/RoI.png "Region of interest"
[Lanelines]: ./test_image_output/lanelines.png "Lane Lines"
[output]: ./test_image_output/output.png "Output"
[original]: ./test_images/solidYellowLeft.jpg "Original"
[blurred]: ./test_image_output/blurred.png "blurred"
---
---

### Lane Finding Pipeline
---
#### Initial Pipeline:

We shall use this image as an example, to explain the various steps that we use to identify the lane lines throught the initial pipeline.
![alt text][original]

My pipeline consisted of 6 steps: 
First, I converted the images to grayscale,
![alt text][gray]


Next, Gaussian Blur is applied to the  grayscale image to smoothen the image. This process helps reduce the outliers in the image and averages out rapid changes in pixel value. 
![alt text][blurred]

Next we use Canny Edge detection to identify strong gradients in the image, which are caused by changes in pixel value(identifying regions of changes in colour). The canny edged detection output is binary image that marks only the pixels there changes are observed, kind of like an outline of all the objects in the image.
![alt text][edges]


Next, to identify the lane lines, we narrow down the field of search, or define a region of interest, which is predominantly the section directly in front if the car.
![alt text][RoI]

Using the Binary image with narrowed area of search, we now use Hough Transformation to identif the exact location of the pixels that form the lanes. these pixel locations are used to identify the lane lines. These locations are then passed through a draw lines function that joins these individual pixels to draw lines on a blank image of the size f the original image,as shown below.
![alt text][Lanelines]

Finally, the image above is superimposed on the original image to give us this output
![alt text][output]

 
#### Update to Pipeline:

The initial pipeline was unable to continuously track the lane line when it encountered broken lines. this meant, if we were to use such a lane finding algorithm in a car, it would periodically lose sight of the lane when encoutering broken lane lines. 

To make the algorithm more robust, the function used to draw lines was modified so that it wuold segregate the points that belonged to the left lane and the right lane, calclate the median slope and intercept of the lines formed for each lane, and use the two extreme points within the region of interest to draw lane lines. 

Also, in case no pixels were detected where lane lines should be present, the algorithm would have memory of its previous lane position, and use that to ensure continuous knowledge of the lane location. This can be done, since from a vehicles perspective, and considering the position of the camera on the vehicle, the changes in lane position will be minute, such that we can expect the lane to be in the same position even if the camera cannot detect it.

For the Challenge video, the presence of shadow and different couloured lines made it difficult to identify the lane lines. to make this detection better, the grayscale image step was replaced with conversion to a HSV image with a combination of the yellow and white colour filters. HSV images are more reliabe in detecting colour differences inspite of presence of shadows. 
 
---
---

### Shortcomings
---
The results of pipline with and without the improvements can be seen in [Test Videos Output](./test_videos_output)

* The main area of improvement would be in terms of robustness of the algorithm. Inspite of the changes, the result was that the detected lane lines were rather shaky, although this may not seem to be as big an issue to look at, since this is the only way the vehicle knows its position respective to the lane, this unstable detection could be an issue for other dependant activities.

Also, the current algorithm cannot detect effectively curved lane lines which the car would encounter during gradual turns on the road.

Finally, the algorithm running on Python is rather slow for real time application, therefore, although this was a good learning experience to start out in this field, I believe there is a a lot of scope of improvement in terms of understanding what issues could occur, and how they can be improved upon.

---
---


### Possible Improvements
---
Having explored a different colour space, I believe alternate colour spaces like the HLS or HSV could be more reliable in terms of detecting lane lines under unpredictable lighting conditions. Also there is a need to improve the robustness of the algorithm, where, using curve fitting techniques may be useful.

Also, in order to improve the speed of the algorithm, maybe the code needs to be more effective, or the platform of implementation needs to be highly powerful. 

---
---

### References:
---
Udacity: Self Driving Car Engineer Nanodegree Project 1


