# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve.jpg.jpg "Solid White Curve"
[image2]: ./test_images_output/whiteCarLaneSwitch.jpg "White car lane switch"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

I took my results from the Hough Quiz as a starting point for my process_frame() method. Then I started adapting my code using the given helper methods to get a clean simple method.
After playing with values for a while, I started thinking about how to adapt draw_lines() to get the lines I needed. My first aproach attempted to avoid just taking all lines and averaging them (which I eventually did end up doing) but rather focus on a different solution extracting information about the set of lines I had. The idea being that the longest line I got (per slope sign) would be the best indication of a lane I should consider as a starting point for my guiding lines. I later found that this solution would require frame-windowing in order to work properly, or at least I think so, since the averaging would not take place using a set of "instant lines" but instead a set of "progressive lines". So the best match I got for one frame would affect how I decide the next one. The final implementation for this version is not included in my code since I didn't find a way to propagate the returned values into the next frame.

You will find two colored lines in my output files
 . one red for the intant averaging solution
 . one red+blue (whatever color that is) for the frame-window averaging solution
 
Watching the videos, the frame-window averaging line is of course very jumpy since there is currently no averaging being made whatsoever. In several cases the slope is similar but the y-intercept jumps or viceversa. If I had cumulative information (say 20 frames? 5 Seconds?) the line should smooth a lot.

![Solid White Curve][image1]
![White car lane switch][image2]
all test images in ./test_images_output/

### 2. Identify potential shortcomings with your current pipeline

As an isolated guiding system, this fails very quickly if the car's bearing changes and the area of interest stops pointing directly into a lane. Long trucks or other objects could cause the lane-line detecting mechanism to have a false positive. The obvious fault cause being the lanes not being painted of course... 

As for my two solutions, I'd be interested in finding out how performant they are in terms of processing speed, since one takes literally every line it finds and starts computing things and averaging values, which the second approach would probably drastically improve since a lot of lines could be discarded, but on the other hand introduces inter-frame information propagation that also requires some processing. This windowed dependency also creates a critical initialization stage when there's no history to consider.

### 3. Suggest possible improvements to your pipeline

If the hardware allows it (in terms of how fast the module needs to respond) I would introduce a segmented area of interest analisis taking a lower bigger area near the car and at least two regions above it to the right and left, to better detect curves. Whatever lines I detect in the upper regions should connect with the projection of the lower area lines, and if they do and the slopes diverge a given amount I could predict a curve.

_____________________________________________________________
As a sidenote, I spent my saturday learning how to GIT and trying to setup my environment. I would have liked to have known that before starting the course so I could have gotten condident with the skills required. I can of course have missed that info, but I don't recall having read anything about setting up our computers to get ready. Would consider that a good improvement to the courses =)
