---
title: "Wii-mote Controlled Differential Wheeled Robot"
collection: projects
type: iRobot Intern Challenge
permalink: /projects/wiimote-create3/ 
# date: 2023-5-8
period: Sep 2023 - Dec 2023
location: 
classes: wide
excerpt: "Controlled a differential wheeled robot with a joystick by establishing bluetooth connections with the robot and joystick and mapping the joystick state to robot wheel speeds."
---
This page is based on the following resources:<br />
<a style="text-decoration: none;" href="https://iroboteducation.github.io/create3_docs/" target="_blank">Documentation <i class="fa fa-file"></i></a><br />
<a style="text-decoration: none;" href="https://github.com/tae-h-yang/create3-with-wii-mote" target="_blank">Code <i class="fa fa-code"></i></a><br />

# Introduction
The goal of iRobot Intern Challenge was to develop a mobile robot that could collect balls and place them in a goal area. Create 3 which was an educational robot produced by iRobot was provided for the development. Create 3 came with a Python SDK that supported bluetooth connection and robot commands. The robot commands included setting wheel speeds, reading sensor data (bumper sensors and processed IMU data), etc. These commands were able to be transmitted from a computer to the robot via Bluetooth. In order to drive the robot remotely, a Wii-mote controller was used. Its joystick controlled the robot to move forward and backward, and rotate.

# Setup
Since <a style="text-decoration: none;" href="https://pypi.org/project/cwiid/" target="_blank">CWiiD</a> was an effective Python package to interface with the Wii-mote, Ubuntu 20.04 was used according to the package's requirement. Additionally, <a style="text-decoration: none;" href="https://github.com/iRobotEducation/irobot-edu-python-sdk" target="_blank">Create 3 Python SDK</a> was intalled.

# Development
The Wii-mote was easily connected to the computer using CWiiD's connection function. Once the controller was connected via Bluetooth, its button and joystick states could be received in real-time. Create 3 was connected to the computer easily as well using its SDK. Once the robot was connected via Bluetooth, the robot commands could be transmitted. The joystick state had to be processed and mapped to proper robot wheel speeds. Then, the calculated speeds were passed to the robot command and sent to the robot. Detailed procedures for establishing the bluetooth connections, reading the joystick state, calculating and sending the robot wheel speeds can be found in this
<a style="text-decoration: none;" href="https://github.com/tae-h-yang/create3-with-wii-mote/blob/main/Create3WithWiiMote.py" target="_blank">repo.</a>  

# Result
<p style="text-align: center;"><iframe width="500" height="200" src="https://www.youtube.com/embed/z335r1G3ISw" title="Drive Create3 with Wiimote Controller" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe><strong>Video 1: Controlling Create 3 with Wii-mote controller.</strong></p>
Video 1 demonstrated the performance of the Wii-mote control for the robot.


