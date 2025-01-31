---
title: "Image to Robot Drawing Trajectory Converter"
collection: projects
type: iRobot Intern Hackathon
permalink: /projects/trajectory-converter/ 
# date: 2023-5-8
period: Jul 2023 - Aug 2023
location: 
classes: wide
excerpt: "Created a program that converted input images (uploaded directly by users or captured with a robot's fisheye camera) to trajectories that the robot would follow to perform its drawing task."
---
This page is based on the following resources:<br />
<a style="text-decoration: none;" href="https://www.diva-portal.org/smash/get/diva2:1679079/FULLTEXT01.pdf" target="_blank">Paper <i class="fa fa-file"></i></a><br />
<a style="text-decoration: none;" href="https://github.com/tae-h-yang/image-to-robot-drawing-trajectory-converter" target="_blank">Code <i class="fa fa-code"></i></a><br />

# Introduction
This project was a part of the robot drawing project using a differential wheeled robot for iRobot Inter Hackathon. The robot was equipped with drawing materials and its purpose was to draw objects in reference images. The images were provided either directly from users or from the robot's fisheye camera which required undistortion. Performing Canny Edge Detection, outlines of the objects in the images were detected and trajectories that the robot should follow while drawing or not drawing were computed connecting the outlines based on closest pixel search. If two outline pixels were far from a certain threshold distance, it was considered as non-drawing area. An additional cascade classifier was used as well in order to detect human faces in the input images and produce better outline results.

# Test Images
<p style="text-align: center;"><img src="/assets/projects/trajectory-converter/profile2.jpg" width="170" height="200" /><img src="/assets/projects/trajectory-converter/fisheye_camera_image.jpg" width="300" height="300" /><strong><br />Fig. 1: Profile image and fisheye camera image for testing.</strong></p>

The images in Fig. 1 were tested to produce drawing trajectories for the user input image and robot's fisheye camera image respectively. For the fisheye camera image, it had to be undistorted first so that the resulting trajectory didn't look different than the actual object. Also, only the desired objects (the human face and logo) in the images needed to be converted to the trajectories. More tested images and results can be found in [Appendix A](#appendix-a-more-testing-images-and-results).

# Image Undistortion
The camera image in Fig. 1 had a barrel distortion. This could be simply undistorted using the camera's intrinsic parameters and distortion coefficients. The parameters and coefficients were provided in advance so camera calibration was not necessary. Detailed procedures for the undistortion can be found in this
<a style="text-decoration: none;" href="https://github.com/tae-h-yang/image-to-robot-drawing-trajectory-converter/blob/main/UndistortImage.ipynb" target="_blank">repo.</a> The resulting undistorted image is shown in Fig. 2 below.
<p style="text-align: center;"><img src="/assets/projects/trajectory-converter/undistorted.jpg" width="300" height="300" /><strong><br />Fig. 2: Undistorted fisheye camera image.</strong></p>

# Object Outline Detection
The input images were blurred using a Gaussian kernel to reduce noises and Canny Edge Detection was performed to compute outlines of the objects in the images. For the profile image, a ROI had to be defined to target only the face unless undesired outlines of the background were computed. The ROI was detected using a cascade classifier model in OpenCV was used. The classifier model was effective in detecting frontal faces. The resulting ROI of the profile image is shown in Fig. 3 below.
<p style="text-align: center;"><img src="/assets/projects/trajectory-converter/profile2_detected.png" width="300" height="300" /><strong><br />Fig. 3: ROI of the profile image detected by the cascade classifier.</strong></p>
The resulting outlines of the input images are shown in Fig. 4 below.
<p style="text-align: center;"><img src="/assets/projects/trajectory-converter/profile2_edge_detected.png" width="225" height="200" /><img src="/assets/projects/trajectory-converter/undistorted_edge_detected.png" width="300" height="300" /><strong><br />Fig. 4: Outlines detected by Canny Edge Detection.</strong></p>
Even though the profile image was cropped based on the ROI, there were still some unwanted edges detected from the background. This could be improved by applying filtering methods (erosion and dilation) and tuning the paramters of Canny Edge Detection. Detailed procedures for the ROI and outline detections can be found in this
<a style="text-decoration: none;" href="https://github.com/tae-h-yang/image-to-robot-drawing-trajectory-converter/blob/main/ImageToTrajectoryConverter.py" target="_blank">repo.</a>

# Path Planning and Trajectory Results
The edges detected were searched through to plan drawing paths. Starting from a edge pixel, its closest edge pixel was searched and connected. If the two pixels were in a specified threshold distance, the connection was considered as a non-drawing area. The resulting drawing trajectories are shown in Fig. 5 below.
<p style="text-align: center;"><img src="/assets/projects/trajectory-converter/profile2_drawing_trajectory.png" width="300" height="200" /><img src="/assets/projects/trajectory-converter/undistorted_drawing_trajectory.png" width="300" height="300" /><strong><br />Fig. 5: Drawing trajectories of the input images.</strong></p>
In Fig. 5, blue lines indicated that the robot should draw while following the paths and red lines indicated that the robot should not draw while following the paths. Detailed procedures for the drawing path planning can be found in this
<a style="text-decoration: none;" href="https://github.com/tae-h-yang/image-to-robot-drawing-trajectory-converter/blob/main/ImageToTrajectoryConverter.py" target="_blank">repo.</a>

# Appendix A: More Testing Images and Results
Other profile and logo images were tested additionally and demonstrated the performance of the trajectory converter.
<p style="text-align: center;"><img src="/assets/projects/trajectory-converter/profile.jpg" width="200" height="150" /><img src="/assets/projects/trajectory-converter/profile_edge_detected.png" width="200" height="150" /><img src="/assets/projects/trajectory-converter/profile_drawing_trajectory.png" width="300" height="150" /></p>
<p style="text-align: center;"><img src="/assets/projects/trajectory-converter/irobot_logo.png" width="300" height="150" /><img src="/assets/projects/trajectory-converter/irobot_logo_edge_detected.png" width="300" height="150" /><img src="/assets/projects/trajectory-converter/irobot_logo_drawing_trajectory.png" width="300" height="150" /></p>