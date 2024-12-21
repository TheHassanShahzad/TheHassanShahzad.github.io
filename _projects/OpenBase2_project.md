---
layout: page
title: OpenBase2
description: A Robust Low-Cost Autonomous robot built from a Hoverboard
img: assets/img/OpenBase2/best.jpg
importance: 1
category: work
---

# What it is

OpenBase2 is a low-cost mobile robotics platform that uses the Nav2 stack alongside ROS2 to act as a robust autonomous robot. It has all the features of A24 as well as the ability to be teleoperated from anywhere using WebRTC communication provided by Transitive Robots. The name OpenBase2 comes from the flexible foundation on which the components are placed and its low-cost design enabling people from everywhere to build autonomous robots that are actually functional. It is based on a hoverboard/Segway and DIN rails commonly used in electrical work, making the components easy to put in and out. More on this later.



## Key Features

- **Autonomous Navigation**: Powered by the Nav2 stack in ROS2 for robust path planning and obstacle avoidance.

- **Flexible Design**: Built using DIN rails and 3030 aluminum extrusions for modularity and easy component swaps.

- **Cost-Effective**: Leverages second-hand hoverboard motors with built-in encoders and a low-cost parts list.

- **Remote Control**: Teleoperable from anywhere using WebRTC.

- **SLAM**: Employs RTAB-Map ROS for mapping and localization, using an Intel D415 depth camera.

- **Hoverboard Motors Integration**: Driven by topic-based ROS2 control using Micro-ROS on an ESP32.



## Parts Used

- Hoverboard motor wheels and battery (\u00a325)

- 2x motor drivers (\u00a315 total)

- DIN rails and clips (\u00a315)

- Raspberry Pi 4 GB (\u00a380)

- 5V 5A step-down converter (\u00a310)

- Depth camera / 2D LiDAR (used for \u00a370)

- 3030 aluminum extrusion (\u00a325)



The total comes down to \u00a3240, which, for a heavy-duty autonomous robot, is on the low end. The bulk of the cost for these robots is typically the battery and motors with encoders; however, all of that comes down to only \u00a325 if you go with second-hand broken hoverboard motors with built-in encoders.



## Hardware Design

The platform is constructed using a combination of DIN rails and 3030 aluminum extrusions. DIN rails provide a robust yet modular foundation to mount components securely while allowing for easy rearrangement or replacement. The 3030 aluminum extrusions contribute to the overall structural integrity and lightweight design, making the robot durable and versatile. This combination ensures that OpenBase2 can be customized to fit various applications or upgraded as needed.



## Software Stack

OpenBase2 uses ROS2 with the Nav2 stack for autonomous navigation and RTAB-Map ROS for SLAM. The navigation system relies on an Intel D415 depth camera to generate maps and localize within the environment. A Raspberry Pi 4 serves as the main computational hub, while an ESP32 running Micro-ROS provides real-time motor control by subscribing to topics for precise operation of the hoverboard motors.



## Challenges Faced

One of the primary challenges was integrating second-hand hoverboard motors with the ROS2 ecosystem. This was addressed by implementing a topic-based ROS2 control system with Micro-ROS on an ESP32, ensuring reliable motor operation. Additionally, optimizing the SLAM algorithm to work seamlessly with the Intel D415 depth camera required careful tuning of parameters to balance performance and accuracy.



## Conclusion

OpenBase2 showcases that robust and functional autonomous robots can be built on a budget without compromising on features. By combining accessible materials like hoverboard motors, DIN rails, and aluminum extrusions with the powerful Nav2 stack in ROS2, OpenBase2 is a testament to innovation and cost-effectiveness in robotics. Stay tuned for more updates and insights into its performance and applications.
