
---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteRight.jpg "Good case"
[image2]: ./test_images_output/solidWhiteCurve.jpg "Bad case"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps: 
* First, I converted the images to grayscale. 
* Then I applied Gaussian blur using blur region of size 5x5. 
* At that point, resulting image is ready for edge detection using Canny transformation which is set to extract bright edges. 
* Now we mask the resulting image with pre-defined region of interest, so that entire image is blacked out, except for tapered area in the middle. This area is where we expect to find lane lines.
* Hugh transformation extracts apparent straight lines among the bright edges in region of interest. These lines mostly match lane lines in some cases such as this one
![alt text][image1]
But when there are other boxy bright objects in the region of interest, we get additional undesired lines like here
![alt text][image2]
* Finally we classify extracted lines by whether they are likely part of left or right lane line. Then we construct 2 straight lines that are best fit for respective groups of extracted Hugh lines. These 2 lines are mixed with original image.

In order to draw a single line on the left and right lanes, I created the draw_fit_lines() function based on draw_lines(). Instead of using slope of input Hugh lines, I found that using position of center of line relative to center of the image gives better results when grouping lines used to construct left and right lane. Then I use linear regression to get left and right line parameters. Finally I bound lane lines so that they stretch from top to bottom of region of interest given their slope and intercept.




###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would is that it assumes no spurious lines from Hugh transformation. Unfortunately this is not the case due other bright objects appearing in region of interest. 

Another shortcoming is that lane lines get curved in turns, so linear fit doesn't work, especially when there are spurious lines coming out of Hugh transformation. 


###3. Suggest possible improvements to your pipeline

A possible improvement would be to use some kind of classifier to eliminate noise from set of lines that are actually lane lines.

Another potential improvement could be to use higher order polynomial fit to address bent lane lines in turns.
