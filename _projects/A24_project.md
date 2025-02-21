---
layout: page
title: A24
description: An Open Source Autonomous SLAM robot for teaching and research based on ROS2, Nav2 and slam_toolbox
img: assets/img/A24/best.jpg
importance: 1
category: Completed 
---

# Building A24: A Low-Cost ROS2 Robot for Learning Navigation

For the better part of the last **three years**, I have wanted to create a robot that moves around my house and serves water to guests. Just the thought of saying, "Robot, bring water to the living room," felt like a distant dream. However, I realized that the tools to achieve this are already available—I just had to learn how to use them. 

The first step was to learn **ROS (Robot Operating System)**, as it provides access to SLAM and pathfinding libraries without requiring me to code them from scratch. I began by creating a smaller version of the robot to mimic the same core functionality as the full-sized version. This approach allowed me to avoid the complexity of managing/paying for more powerful motors and figuring out how to carry water effectively.

Thus began **A24**, a **low-cost** robot designed to teach myself the **Nav2 stack**. While I could have done everything in simulation, I'm glad I didn’t because there is a significant disparity between building a SLAM robot in simulation versus real life. The lessons learned from working on a physical robot were invaluable.

---

# The Hardware Design

## **Three Stages**

**A24** is composed of three laser-cut plates made from a single sheet of strong 6mm transparent acrylic:

### **1. Lower Stage**
The `lower stage` houses:
- **Two sensored DC motors**.
- A **LiPo battery**.
- A low-cost caster wheel.
- **3D-printed mounts** to ensure the robot's LiDAR remains parallel to the ground.

### **2. Middle Stage**
The `middle stage` was designed for **modular component organization**. It features a grid of 3.5mm holes, which hold the following components:
- A **Raspberry Pi 4**.
- A **step-down converter**.
- An **L298N motor driver**.
- A hardware driver board built using a **Seeeduino Xiao**, outfitted with male header pins and PCB screw terminals to interface with the encoders and motor driver.
- A **power distribution board**, essentially a set of screw terminals wired together to separate voltage channels.

Each component is secured with custom **3D-printed mounts**.

### **3. Upper Stage**
The `upper stage` supports:
- An **RP-LiDAR A1**.
- A custom connector for the LiDAR's small PCB, allowing connection via a microUSB cable for communication.

---

## **The Display and Switch Board**
Between stages 1 and 2, I added a **3D-printed piece** that houses:
- A **display** to monitor the battery levels and voltage supplied by the LiPo.
- An **SPST rocker switch** to turn the robot on and off.

---

## **Connecting the Plates: Custom Legs**
Initially, I planned to use brass standoffs to connect the plates, but the ones I had were **male-female** instead of **female-female**. The laser cutter also made the holes too large to screw into directly, rendering this approach unfeasible. 

Instead, I designed custom **hexagonal standoffs** with:
- **Holes for threaded brass inserts** at the top and bottom.
- A central hole allowing the use of any length of M3 screw.

To secure the brass inserts, I used a soldering iron to press them into place. This solution is highly adaptable—the legs' height and diameter can be adjusted based on user preferences.

---

# The Software Design

## **fusion2urdf for ROS2**
The first step was to use the [fusion2urdf ROS2 plugin](https://github.com/dheena2k2/fusion2urdf-ros2). This plugin converts my Fusion 360 model into a **URDF** (Unified Robot Description Format) file for use in simulation. The URDF is essential because:
1. It allows the **pathfinding algorithm** to understand the robot's collision bounding box.
2. It provides SLAM software with the necessary transformations between the sensor's origin and the robot's origin.

The package also includes ready-to-use ROS2 description files, such as `display.launch.py` for RViz and `gazebo.launch.py` for Gazebo. While typically the bringup and description packages should be separate, combining them was acceptable for initial testing.

---

## **SLAM with ROS2**
Mobile robots in ROS2 determine their **odometry** (where the robot thinks it is) using their kinematics and state feedback. Adding sensors, such as an **IMU** for position tracking, can improve odometry, but it will still drift over time unless corrected. 

This is where **SLAM (Simultaneous Localization and Mapping)** comes into play. Tools like **slam_toolbox** update the transform between the global frame (the map's origin) and the robot's odometry frame. Notably:
- The same code can be used for simulation and real-world applications by simply toggling the `use_sim_time` parameter.
- ROS's architecture enables **Sim2Real** transitions seamlessly since nodes are **environment-agnostic**.

---

## **microROS with ROS2_control**
To update the robot's odometry and execute motion commands, I used **ROS2_control**. Initially, the learning curve was steep, but the process became clear once I understood the basics:
- **JointStateBroadcaster**: Updates the transforms based on encoder feedback.
- **DiffDriveController**: Computes velocity commands for each wheel based on the desired trajectory (via the `cmd_vel` topic) and updates the robot's odometry.

### **microROS Integration**
Typically, robots require custom hardware components and protocols for communication with ROS2_control. However, I opted for **microROS**, which simplifies communication by enabling microcontrollers to act as ROS nodes. 

Using an **Seeeduino XIAO** running microROS:
- The microcontroller can **publish** and **subscribe** to ROS topics like any internal node on the main computer.
- An additional node acts as a **translator**, converting the JointState messages to `Float64MultiArray` messages, which contain only the essential data needed by the Seeeduino Xiao.

This **topic-based approach** simplifies the code, reduces system complexity, and improves efficiency since `Float64MultiArray` is smaller than `JointState`.

---

Building A24 has been an incredible learning journey, from hardware assembly to mastering the ROS2 ecosystem. By starting with a small, functional prototype, I’ve not only gained valuable hands-on experience but also laid the groundwork for more ambitious robotics projects in the future.



<!-- For the better part of the last 3 years of my life i wanted to make a robot that moves around in my house and gives water to the guests. Just the thought of saying "robot bring water to the living room" felt like a distant dream but i realized that the tools to make this are already available and all i had to do was learn how to use them. The first step to make this was learn ROS since that gave me access to SLAM and path finding libraries without needing to code them myself. I began by making a small version of the robot that would mimic the same core functionallity as the bigger robot without the hassle of managing/paying for more powerful motors and carrying around water somehow.

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


# SLAM with ROS2
Essentially mobile ROS2 robots work by using their kinematics and state feedback to obtain their odometry- where the robot thinks it is. Odometry can be improved by adding more sensors, for example doing position tracking with aan IMU however it will always drift with time unless some algorithm attempts to update where the robot thinks it is. That is SLAM. Packages like slam_toolbox update the transofrms between the newly created global frame (the maps origin) to the robots odometry (where the robot think it is). The code to do this in simulation is the same as in real life but you just have to change the use_sim_time parameter which is the beauty of ROS architecture. Sim2Real done autonamtically as the nodes are environment agnostic. They dont cae if they are in a simulation or in real life.

# microROS with ROS2 control
In order to update the robots odometry and for the robot to actually move based on the conteollers output command, I use ROS2 control. At first it took some time to get my head around it but in reality it isnt complicated. I specify 2 basic controllres in my controllers.yaml file: JointStateBroadcaster and DiffDriveController. The first updates by transofrms depending on the state of the encoders and the second finds out what commadn velocity to give each wheel depending on what my desired trajcetory is (using the cmd_vel topic). The diff drive controller also updates my odometry based on the received velocity of the wheels.

But the question is how does my microcontroller communicate with ros2_control? the typical answer would be to make a custom hardware component and have a custom protocol to send and receive data however that solution is out dated and time consuming. Instead i leveraged microROS. microROS is a ros stack dedicated to devices not powerful enough to use the full capability of ROS so its intended for microcontrollers. I wont get in to the details of microROS but by having an agent node running on your main computer, the esp3 can have access to to the computers internal nodes. It can publish do everything you can do internally on your computer but just on a microcontroller. 

The problem is that ros2 control does not out of the box make use of this topic based control. Most robots have to use serial communication however there is a little trick which is topic based ros2 control. I honestly hardly hear this being used and i think people are wasting their time by not using this. It allws the microcontroller to make use of ros2 control by only publishing and subscribing to data. To make it work i have an extra node running on the computer that acts as a translator or reducer; it takes in te joint state messgaes running on ros2 and converts them to FLoat64MultiArray with only the bare essential data for seeduino xiao to publish and subscribe to. This makes the code easier to writw and i think speeds up the system since a JointState message can be larger than a Float64 array. -->

<!-- # The ROS2 design
My design was based on using microROS within the Seeeduino xiao which did 2 things:
- Publish its encoder count data to `/encoder_data`
-Subscribe to `pwm_data` to get pwm values that each motor should spin at

The other nodes on my ROS2 network handled odometry, coming with the pwm values to move the robot based on the robots paramaters and the slam_toolbox/Nav2 packages

## The illegal shortcut i came up with
Now normally after getting to this stage you are meant to look into ros2 control but that was based on serial communication between the computer and microcontroller to send and receive commands. I decided to go against this as my solution was based on microROS and the ros2_control didnt have lots of documentation or tutorials to go along by.

Instead i decided to write a custom node to handle odometry and publish it to `/odom` aswell as handle the transforms the 2 wheeled robot would make with the maps origin. This node was based on subscribing to the encoder data and calculating distance travelled by looking at the changes in encoder count every iteration and altering the displacement travelled by the robot. 

To make this code even easier, i just set the `base_link` to be the centre of the wheels axis of rotation and not the centre of the robot itself which would have been another unecessary translation. 

The only transform needed would be how the `base_link` maps to the centre of the lidar which was easily calculated in CAD.

After updating the URDF file to encompass these changes I was left with a bare bones basic urdf where the entire mesh of the robot was one file and the collision avoidance was a rectangular block to speed up navigation. 

The odometry node also calculated the `linear.x` and `angular.z` based on encoder data to know the robots velocity.  -->
