---
layout: page
title: A24
description: An Open Source Autonomous SLAM robot for teaching and research based on ROS2, Nav2 and slam_toolbox
img: assets/img/A24/best.jpg
importance: 1
category: work
---

For the better part of the last 3 years of my life i wanted to make a robot that moves around in my house and gives water to the guests. Just the thought of saying "robot bring water to the living room" felt like a distant dream but i realized that the tools to make this are already available and all i had to do was learn how to use them. The first step to make this was learn ROS since that gave me access to SLAM and path finding libraries without needing to code them myself. I began by making a small version of the robot that would mimic the same core functionallity as the bigger robot without the hassle of managing/paying for more powerful motors and carrying around water somehow.

Thus began A24, a as-low-cost as possible robot to teach myself the Nav2 stack. I could have done it all in simulation but im glad i didnt because there is a very large disparity between making a SLAM robot im simulation and in real life and the lessons taught were more than valuable.

# The Hardware Design
## 3 stages
A24 is composed of 3 laser cut plates cut from a single sheet of strong 6mm see through acrylic. The `lower stage` holds the 2 sensored DC motors and the LiPo battery. Attached to the plate is also a low cost castor wheel while some 3d printed mounts to make the sure the robots lidar stays parallel to the ground

The middle stage was designed to make component organizing be as modular as possible. It consists of a grid of 3.5mm holes upon which i mounted the `Raspberry Pi 4`, Step down converter , `L298N` motor driver and hardware driver board made up of a `seeduino xiao` with lots of male header pins and PCB screw terminals to interface with the encoders and motor driver. There is also a power distribution board which is just some screw terminal wired together to seperate the voltage channels. Each components was secured witha  custom 3d printed mount

The upper stage holds the RP-Lidar A1 lidar and a custom connector to attach the lidars small PCB through which you connect a microUSB capable to and communicate with the lidar

## The Display and switch board
Between stages 1 and 2 I also have a 3d printed piece that houses a display to monitor the battery levels and voltage coming through the LiPo. On the piece theres also a SPST rocker switch to turn the entire robot on and off.

## The legs to connect the plates together
Initially I planned to use brass standoffs to connect the plates together but the ones i had were male female instead of female female. I thought i could screw the male part into the laser cut part but that wasnt an option given the laster cutter made the whole too big. I think its a good thing as that wasnt a good solution and my next choice didnt require having to buy standoffs and could be modifified based on user preference.

I designed hex standoff with holes for 2 threaded brass inserts at the top and bottom and hole going straight through. The threaded brass inserts were put in place using a soldering iron keeping it firm in place. The hole going straight through meant i can use any length M3 screw i choose to. This solution is nice as the person can easily change the diamater of the legs to make them more durable or change the height depending on how far apart they want their plates to be.

# The Software Design
## fusion2urdf ros2
The first step was to use this <a href="https://github.com/dheena2k2/fusion2urdf-ros2">fusion360 plugin</a> called `fusion2urdf`. This allowed my fusion360 model of the robot to become a urdf for use in simulation and so the path finding algorithm knows my collision bounding box. Its also useful for SLAM so the software knows how the sensors origin translate to the robots origin

This package also gave me a ready to go ros2 description package containing the `display.launch.py` files and `gazebo.launch.py` files for RViz and gazebo respectively. Nrmally this should be split into 2 packages so the bringup is seperate from description but for testing purposes this was fine.

# The ROS2 design
My design was based on using microROS within the Seeeduino xiao which did 2 things:
- Publish its encoder count data to `/encoder_data`
-Subscribe to `pwm_data` to get pwm values that each motor should spin at

The other nodes on my ROS2 network handled odometry, coming with the pwm values to move the robot based on the robots paramaters and the slam_toolbox/Nav2 packages

## The illegal shortcut i came up with
Now normally after getting to this stage you are meant to look into ros2 control but that was based on serial communication between the computer and microcontroller to send and receive commands. I decided to go against this as my solution was based on microROS and the ros2_control didnt have lots of documentation or tutorials to go along by.

Instead i decided to write a custom node to handle odometry and publish it to `/odom` aswell as handle the transforms the 2 wheeled robot would make with the maps origin. This node was based on subscribing to the encoder data and calculating distance travelled by looking at the changes in encoder count every iteration and altering the displacement travelled by the robot. 

To make this code even easier, i just set the `base_link` to be the centre of the wheels axis of rotation and not the centre of the robot itself which would have been another unecessary translation. 

The only transform needed would be how the base_link maps to the centre of the lidar which was easily calculated in CAD.

After updating the URDF file to encompass these changes I was left with a bare bones basic urdf where the entire mesh of the robot was one file and the collision avoidance was a rectangular block to speed up navigation. 

The odometry node also calculated the `linear.x` and `angular.z` based on encoder data to know the robots velocity. 
