---
layout: page
title: BiStable
description: A 2 wheeled self-balancing hand following and RC ROS2 robot
img: assets/img/BiStable/incline.jpg
importance: 1
category: work
---

For my Year 1 undergarduate course we were assigned as a group to make anything related to electronics for our 2nd term end of module project. While we could have gotten away with connecting a few sensors to a MCU and gotten a high mark, we collectively decided to make something that pushed our abilities greatly and led us to making something that at the time we werent sure how to make.

We said, let's make a 2 wheeled self balncing robot that not only utillized control algorithms to stabllise but the tuning could be done from the same bluetooth gamepad remote that controlled it. And make the whole thing based upon the ROS2 frameowrk allowing for future scalabillity and easier additional nodes to be written for the robot. And if that wasnt enough we said lets put a camera on the robot and make it "hand following". If the robot sees a hand in frame, it will begin following the hand mantaining a set distance apart.

This project was made start to finish in around 2 weeks so for the time constraints of the project, we were all very happy with what was achieved

Anyway lets get into what the robot was composed of and technical challenges faced

## The Robots Hardware
#### The Robot included the following components:

- Raspberry Pi 4 8GB
- ESP32 WROOM32 dev board
- 2 x NEMA17 steppter motors
- 2x DRV8825
- 0.96 inch OLED Display
- 5V 5A step down converter
- 3S LiPo battery
- External EEPROM chip
- MPU6050 6-axis IMU
- 2020 aluminium extrustion
- RGB USB camera

The Raspberry Pi was responsible for running ROS2, connecting to the gamepad and camera, sending custom messages to the esp32 to tell it what inclination it should be at and what its rotational velocity should be and finally what the PID gains are. More on this later

The ESP32 was the MCU responsible for stabillising the robot by reading data from the MPU6050, driving the stepper motors and writing to EEPROM

The stepper motors were used as it has high accuracy and precision while being sensorless however velocity control was found challenging while trying to execute code in the same loop. For more info fo to the section called `main challenges`

Aside from that was the stepper drivers and basic electronic components like the MPU6050 IMU which is a popular choice for projects requiring an IMU, step down converters capable of supplying enough current and an EEPROM chip for saving the PID gains without needing to retune every time the robot is turned on. Information about the PID and robots inclination is found using the OLD display

For the spine of the robot we went with 2020 alumnium due to its abillity to easily mount and re-arrange components rapidly. This meant we could quickly begin programming the robot without worrying too much on the CAD itself or wasting time having to re-print parts

# How everything worked
## The ROS side
I want to start with the ROS side of things because before the MCU even knows how to stabillise, it needs to receive a message from the RPi telling it what angle it should be at and its rotational velocity

The robot has 4 packages that each serve a different purpose:
- BiStable_scripts
- BiStable_description
- bistable_interfaces
- bistable_bringup

`bistable_interfaces` contains the message types for the rest of the ROS2 nodes. It contains 2 message types:

```liquid
GamePadData.msg
float32 inclination
float32 yaw_vel
float32 eq_change
float32 kp_change
float32 ki_change
float32 kd_change
bool tracking_toggle
```
and for the hand tracking

```liquid
TrackingData.msg
float32 inclination
float32 yaw_vel
```

`BiStable_scripts` is the main package that contains the nodes for the robot. It contains the following nodes:
- `gamepad_publisher`
  - publishes the joystick values mapped from -1 to 1 for x and y
  - button presses as 0's and 1's for each button and trigger
  - Published using the built in message type `Int16MultiArray`
- `gamepad_publisher`
  - subscribes to the 
  
