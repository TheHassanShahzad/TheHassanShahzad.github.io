---
layout: page
title: RoboPipe
description: Control Simulated Robot Arms with just a Hand and a Laptop Webcam
img: assets/img/RoboPipe/best.jpg
importance: 2
category: work
---

<div class="col-sm mt-3 mt-md-0">
    {% include video.liquid path="https://www.youtube.com/embed/7H_1pL-8Ow8" class="img-fluid rounded z-depth-1" %}
</div>

In 2023 I attended the Smart Green makeathon with a few of my UCL robotics and AI classmates. The event was what i call an Engineers playground. We had lots of tools and expensive devices at our disposal with the aim to make something new. One of the tools were could use was an IGUS robot arm and given we do Robotics and AI, that felt like the perfect thing to use. 

The problem was what to do with it. What problem could we solve that actually helped people. That was when it struck me, what if we made a software to control robot arms like this one with the aim to reduce the operational complexity. You see industrial robot arms are born to make precise and calculated movements to get from point A to point B but the people who own the factories have no chocie but to hire people in order to program the robots and get them to do things.

Why should the software to control robot arms be so complex? What if you could use something that everyone uses to already pick and place objects and have that teach the arm. The hands!

The idea was simple, we use the laptops webcam and 2 hands to control the robot arm aswell a gripper. Long story short we partnered with a team from IIT, the best engineering university in India and the most competetive in Asia, and we made it happen. But thats for anothe rproject post. The problem was when we finished, we found the software to be slow and was catered especially to the needs of IGUS with no ROS involved. This was a problem which I intended to solve after arriving back home

RoboPipe is a ROS2 library that is ported to PyBullet and it allows one hand to control the position of an end effector for a robot. The package is using the KUKA iiwa but ideally it should suport any robot arm

