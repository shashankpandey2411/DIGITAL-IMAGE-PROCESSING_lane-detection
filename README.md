# DIGITAL-IMAGE-PROCESSING_lanedetection



![image](https://user-images.githubusercontent.com/120237181/206841040-ba1cf92e-a044-49ae-874c-87f37c9a1968.png)






Lane detection is a developing technology that is implemented in vehicles to enable autonomous navigation. Most lane detection systems are designed for roads with proper structure relying on the existence of markings. The main shortcoming of these approaches is that they might give inaccurate results or not work at all in situations involving unclear markings or the absence of them. In this study one such approach for detecting lanes on an unmarked road is reviewed followed by an improved approach. Both the approaches are based on digital image processing techniques and purely work on vision or camera data. The main aim is to obtain a real time curve value to assist the driver/autonomous vehicle for taking required turns and not go off the road.

This project is created to demonstrate how a lane detection system works on cars equipped with a front-facing camera. Finding a place in more and more vehicles, this system is an essential part of the advanced driver assistance systems (ADAS) used in autonomous/semi-autonomous vehicles. This feature is responsible for detecting lanes, measuring curve radius, and monitoring the offset from the center. With this information, the system significantly improves safety by making sure the vehicle is centered inside the lane lines, as well as adds comfort if it is also configured to control the steering wheel to take gentle curves on highways without any driver input. This is a simplified version of what is used in production vehicles, and the best functions if good conditions are provided (clear lane lines, stable light conditions). In this repository, it is included a dash cam footage for the script to work.

**Methodology **


After overall look up of so many techniques we choose a feasible, efficient, effective methodologies and algorithms we 
prepared a pipeline structure for the implementation part. Lane lines on roads are actually in white colour and yellow. 
We need to select the most fitting color space that clearly highlights the lane lines. We decided to apply HSL color 
selection because after researching all we found out that using HSL will be the best colour space to use and we mask 
their yellow and white color space. Then we apply Canny Edge detection for edge detection and the algorithm has been 
already explained in short above. We're interested in the area facing the camera, where the lane lines are found. So, we'll 
apply region of interest to cut out everything which we not required else

**WORKING :**

Image Processing

1.readVideo ()

● First up is the readVideo () function to access the video file which is located in the same directory.

2.processImage ()

● This function performs some processing techniques to isolate white lane lines and prepare it to be 
further analysed by the upcoming functions. Basically, it applies HLS color filtering to filter out whites 
in the frame, then converts it to grayscale which then is applied thresholding to get rid of unnecessary 
detections other than lanes, gets blurred and finally edges are extracted with cv2.Canny() function. The 
canny edge detection first removes noise from image by smoothening. It then finds the image gradient 
to highlight regions with high spatial derivatives.

3.perspectiveWarp ()

● Now that we have the image we want, a perspective warp is applied. 4 points are placed on the frame 
such that they surround only the area in which lanes are present, then maps it onto another matrix to 
create a Birdseye look at the lanes. This will enable us to work with a much-refined image and help 
detect lane curvatures. It should be noted that this operation is subject to change if another video is 
used. The predefined 4 points are calculated with this particular footage in mind. It should be returned 
if another video has a slightly different angled camera.

Lane Detection, Curve Fitting 

1. Plot Histogram ()

● Plotting a histogram for the bottom half of the image is an essential part to obtain the information of 
where exactly the left and right lanes start. Upon analysing the histogram, one can see there is two 
distinct peaks where all the white pixels are detected. A very good indicator of where the left and right 
lanes begin. Since the histogram x coordinate represent the x coordinate of our analysed frame, it 
means we now have x coordinates to start searching for the lanes.

2. Slidewindow Search ()

● A sliding window approach is used to detect lanes and their curvature. It uses information from the 
previous histogram function and puts a box with lane at the center. Then puts another box on top based 
on the positions of white pixels from the previous box and places itself accordingly all the way to the 
top of the frame. This way, we have the information to make some calculations. Then, a second-degree 
polynomial fit is performed to have a curve fit in pixel space.

3. General Search ()

● After running the slide_window_search () function, this general_search () function is now able to fill 
up an area around those detected lanes, again applies the second-degree polyfit to then draw a yellow 
line which overlaps the lanes pretty accurately. This line will be used to measure radius of curvature 
which is essential in predicting steering angles.

4. measure_lane_curvature ()

● With information provided by the previous two functions, np. polyfit () function is used again but with 
the values multiplied by xm_per_pix and ym_per_pix variables to convert them from pixel space to 
meter space. xm_per_pix are set as 3.7 / 720 which lane width as 3.7 meters and left & right lane base 
x coordinates obtained from histogram corresponds to lane width in pixels which turns out to be 
approximately 720 pixels. Similarly, ym_per_pix are set to 30 / 720 since the frame height is 720.

Visualization and Main Function 

 1. draw_lane_lines ()
 
● From here on, some methods are applied to visualize the detected lanes and other information to be 
displayed for the final image. This particular function takes detected lanes and fills the area inside them 
with a green color. It also visualizes the center of the lane by taking the mean of left_fitx and right_fitx 
lists and storing them in pts_mean variable, which then is represented by a yellowish color. This 
variable is also used to calculate the offset of the vehicle to either side or of it is centered in the lane.

2. offCenter ()

● Off-Center() function uses pts_mean variable to calculate the offset value and show it in meter space.

3. addText ()

● Finally, by adding text on the final image would complete the process and the information displayed.

4. main ()

● Main function is where all these functions are called in the correct order and contains the loop to play 
vide





![image](https://user-images.githubusercontent.com/120237181/206842433-cac6c88d-9d94-48b8-b336-11c00a797e0a.png)

