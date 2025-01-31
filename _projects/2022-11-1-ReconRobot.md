---
title: "Autonomous Reconnaissance Robot"
collection: projects
type: 
permalink: /projects/recon-robot/ 
# date: 2023-5-8
period: Nov 2022 - Dec 2022
location: 
classes: wide
excerpt: "Employed Gmapping SLAM, Frontier-Based Exploration and tag detection on TurtleBot3 to achieve a reconnaissance mission autonomously in a closed and unkown environment."
---
Some of the contents in this page is based on the following resource:<br /> 
<a style="text-decoration: none;" href="/assets/projects/recon-robot/final-report.pdf" target="_blank">Report <i class="fa fa-file"></i></a><br />
<!-- <a style="text-decoration: none;" href="https://emanual.robotis.com/docs/en/platform/turtlebot3/overview/" target="_blank">TurtleBot3 <i class="fa fa-external-link-alt"></i></a> -->

# Introduction
Amid disaster response scenarios, the inherent danger and operational challenges often render it impractical for first responders to execute missions effectively. This situation highlights the indispensable role of autonomous robots as viable assets in such contexts. This project aimed to integrate autonomous navigation and object detection onto the open-source robot platform, TurtleBot3. Its objective was to conduct a reconnaissance mission, enabling the robot to map closed and unknown environments while identifying and pinpointing the poses of victims represented by AprilTags.

# Robot Hardware Setup
<p style="text-align: center;"><img src="/assets/projects/recon-robot/turtlebot3_burger_components.png" width="500" height="500" /><strong><br />Fig. 1: TurtleBot3 components <a href="#1">[1]</a>.</strong></p>
The robot comprised of components in Fig. 1 and an additional Pi camera was mounted and connected to the SBC to detect AprilTags as shown in Fig. 2 below.
<p style="text-align: center;"><img src="/assets/projects/recon-robot/pi-camera.png" width="300" height="300" /><strong><br />Fig. 2: Pi camera mounted on TurtleBot3.</strong></p>

# Robot Software Setup
In order to implement the sensors on the robot and create an effective development environment, several setups were required. First, a personal computer needed to have Ubuntu 20.04 and ROS Noetic installed for running ROS Master remotely. Dependant ROS packages and TurtleBot3 packages were installed as well. Secondly, the SBC's microSD was burned with Raspberry Pi ROS Noetic image. Thirdly, required packages for the OpenCR firmware were installed on the SBC and uploaded to the OpenCR. Lastly, intrinsic camera calibration of the Pi camera was performed using a checkerboard. More detailed setup tutorials can be found <a style="text-decoration: none;" href="https://emanual.robotis.com/docs/en/platform/turtlebot3/quick-start/#pc-setup" target="_blank">here.</a><br />

# Autonomous Navigation
<p style="text-align: center;"><img src="/assets/projects/recon-robot/explore-lite.png" width="800" height="300" /><strong><br />Fig. 3: ROS node architecture of <i>explore_lite</i> package <a href="#2">[2]</a>.</strong></p>
The robot navigated a closed and unknown environment with the ROS packages in Fig. 3. SLAM was achieved by Gmapping SLAM with the LiDAR on the robot and it generated an occupancy grid map of the environment. <i>explore_lite</i> package implemented Frontier-Based Exploration based on the occupancy grid map and published movement commands to <i>move_base</i> node which calculated and controlled accurate robot wheel speeds from the movement commands. 

# AprilTag Detection
<p style="text-align: center;"><img src="/assets/projects/recon-robot/tag-detection.png" width="800" height="300" /><strong><br />Fig. 4: ROS node architecture of <i>apriltags2_ros</i> package <a href="#3">[3]</a>.</strong></p>
AprilTags were detected by the Pi camera with the ROS package in Fig. 4. The intrinsic camera parameters calculated from the software setup were used to calculate poses of the tages with respect to the camera frame. Extrinsic camera calibration was not necessary since the size of the tags were known and incorporated in the ROS package.

The poses of the tags had to be transformed from the camera frame to the map frame so that the tags could be displayed on the occupancy grid map. The transformation matrix between the camera frame and robot frame was calculated by accurately measuring the location of the camera on the robot. The other transformation matrices were provided by <i>apriltags2_ros</i> and Gmapping SLAM node.

# Reconnaissance Mission Result
A closed environment with AprilTags attached on the walls was created as shown in Fig. 5.  
<p style="text-align: center;"><img src="/assets/projects/recon-robot/test-environment.jpg" width="300" height="300" /><strong><br />Fig. 5: Testbed for robot's reconnaissance mission.</strong></p>
The robot conducted its reconnaissance mission in two phases. In the first phase, the robot explored the test environment efficiently with the navigation nodes and generated an occupancy grid map of the environment as shown in Fig. 6
<p style="text-align: center;"><img src="/assets/projects/recon-robot/occupancy-grid-map.png" width="300" height="300" /><strong><br />Fig. 6: Occupany grid map of testbed generated by robot.</strong></p>

<p style="text-align: center;"><iframe width="415" height="200" src="https://www.youtube.com/embed/7zTOPSlm9X4" title="TurtleBot3 Frontier-Based Exploration" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe><strong>Video 1: Running Frontier-Based Exploration on TurtleBot3.</strong></p>

<p style="text-align: center;"><iframe width="1512" height="696" src="https://www.youtube.com/embed/0K5MpHrOGu4" title="Turtlebot3 Frontier-based exploration displayed on Rviz" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe><strong>Video 2: Frontier-Based Exploration displayed on Rviz.</strong></p>
Video 1 and 2 showed demonstrations of the first phase of the reconnaissance mission in the testbed and on Rviz.

In the second phase, the robot concentrated on locating the tags and returning their poses on the map generated in Fig. 6. Given the map, every wall of the testbed was scanned by the robot. Since the camera was mounted on the right side of the robot, the walls which were occupied grids in the map could be scanned thoroughly by navigating the robot along the walls in a certain direction. Whenever the tags appeared in the camera, their poses were transformed to the map frame and recorded as shown in Fig. 7 below.

<p style="text-align: center;"><img src="/assets/projects/recon-robot/tag-poses.jpg" width="300" height="300" /><strong><br />Fig. 7: Poses of tags displayed on occupancy grid map.</strong></p>
Some poses of the tags were not on the walls accurately and this was due to drift errors of the robot poses after driving for a while.

<p style="text-align: center;"><iframe width="512" height="239" src="https://www.youtube.com/embed/P_0O_4yj3Lw" title="Turtlebot3 Apriltag Detection" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe><strong>Video 3: Running AprilTags detection on TurtleBot3.</strong></p>
<p style="text-align: center;"><iframe width="512" height="239" src="https://www.youtube.com/embed/7F1edCN-DYA" title="Turtlebot3 Apriltag Detection displayed on Rviz" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe><strong>Video 4: AprilTags detection displayed on Rviz.</strong></p>
Video 3 and 4 showed demonstrations of the second phase of the reconnaissance mission in the testbed and on Rviz. All of the tags in the testbed were detected and displayed on the map. Therefore, the robot successfully executed its mission. 


# References
<a name="1"></a>[1] ROBOTIS e-Manual, "TurtleBot3 Specifications," Available: https://emanual.robotis.com/docs/en/platform/turtlebot3/features/#specifications.<br />
<a name="2"></a>[2] ROS Wiki, "explore_lite," Available: http://wiki.ros.org/explore_lite.<br />
<a name="3"></a>[3] ROS Wiki, "apriltag_ros," Available: http://wiki.ros.org/apriltag_ros.<br />
