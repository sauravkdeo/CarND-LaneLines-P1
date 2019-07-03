# **Finding Lane Lines on the Road**

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/gray_solidYellowLeft.jpg "Grayscale"
[image2]: ./test_images_output/blur_gray_solidYellowLeft.jpg "Grayscale_Blur"
[image3]: ./test_images_output/canny_image_solidYellowLeft.jpg "Canny"
[image4]: ./test_images_output/masked_image_solidYellowLeft.jpg "Canny_masked"
[image5]: ./test_images_output/line_img_solidYellowLeft.jpg "Line"
[image6]: ./test_images_output/final_solidYellowLeft.jpg "Result"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps.

`Step 1 : Grayscale `

The RGB image is converted to grayscale image.

![alt text][image1]

`Step 2 :Noise Reduction `

The grayscale image is processed using GaussianBlur to reduce the noise and smoothen the image.kernel size =3 was used for denoisining.

![alt text][image2]

 `Step 3 :Edge Detection`

 Once the gracscale image is smoothened, Canny transform is applied on the smoothened grayscale image to detect the edges in the images.Low threshold = 50 & high threshold = 150 were used for edge detection.

 ![alt text][image3]

 `Step 4 :Image Masking`

 We know that the the lanes will always be in the specific area of the image. Hence we used masking to keep only the lane specific area after edge detection. All other areas apart from the lane region were removed.

![alt text][image4]

 `Step 5 : Hough Transformation`

 The masked image was processed and used to detect the edge coordinates in pixel.Using Hough transformation, staring and end coordinates of the all the edges were returned in the form of array.

 ![alt text][image5]

 `Step 6 : Draw Lines`

 Once we had the set of coordinates representing the lines. The first task was to separate left lane coordinate set from right lane coordinate set.
 I used the slope information to separate these two, as left lane lines will have negative slope and right lane line will have positive slope. I stored the the set of left lane lines and right lane lines in two separate arrays.

 For each lane lines, we calculated the weighted mean slope and intercept.

 We used this information to draw the lines on the original image.

 ![alt text][image6]



### 2. Identify potential shortcomings with your current pipeline

The current pipeline has several shortcoming and limitations.

* This algorithm is hardcoded to mask certain region.If the front are not in that region, the algorithm will not work properly.
* The algorithm will only work fine for the straight lanes.On curved paths, there is a possibility that the slopes of the left lanes and right lane lines may be same, leading to improper identification of the lane.
* The algorithm will only work fine, when the brightness and the contrast of the image is good, as the algorithm is based on the edge detection technique.


### 3. Suggest possible improvements to your pipeline

* Pre-process the image to adjust the contrast and brightness of the image to properly detect the edges.

* Use homography to view the image from top before edge detection(canny transformation)

* Use Image from an infrared camera

* Adding an outlier reduction approach like RANSAC on the hough lines.

* Using curve fitting to plot the curve instead of straight lines
