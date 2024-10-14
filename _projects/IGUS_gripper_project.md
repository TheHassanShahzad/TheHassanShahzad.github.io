---
layout: page
title: a custom gripper for the IGUS robot arm
description: ade at the SMart Green makeathon in Gran Canaria with the aim to control an industrial robot arm with nothing but your hands and a laptop webcam
img: assets/img/IGUS_gripper/IMG_2144.jpg
importance: 2
category: work
---

I took part in the Smart Green makeathon in 2023 with a few other classmates from my course and we worked together on a project that we believed would be beneficial for the future of human robot interaction.

The problem we came up with was that factory owners have no choice but to hire people in order to automate tasks and one of the leading reasons as to why was that the software was too complex to get started.

We invented a software that allowed the IGUS arm to be totally controlled using 2 hands, where one hand was for moving the end effector to an x,y,z position and the other hand was responsible for opening and closing the gripper itself. The entire robot from idea to testing was completed in only 3 days with little to no sleep. So for the time constraints, I am extremely happy with what we came up with

# The arm itself
When we first walked into the hall for the competition, we honestly were not anticipating anything incredible. I had a feeling it would be more of a conference or they would give us some ready made kits to program but thats when i say a table with black and orange robot arms laid out. From that momement on i decided to give this event more than a little effort but my best. The opportunity to use an arm i wouldnt get access to for who knows how many years and make it perform an innovative task was staring right at me. I couldnt turn it down.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/IGUS_gripper/IMG_2046.jpg" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/IGUS_gripper/IMG_2047.jpg" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
</div>

After discussing how we can use the arm as a group I stumbled on the idea that we could control it with our hands a webcam. I hadnt seen something like this ever done before but only in the setup where the user is wearing expensive gloves. After persuading the team and finding partners to work with from IIT we got to work to lay out the objectives.


We needed to know what was the most intuitive way to control the arm with just hands and that itself took a day. We first began by looking at how the shoulder could be the orign of the arm and we use the hand moving relative to the shoulder as a means to control the arm but that idea fell aprt quickly. After literally hours of discussion working through every nook and cranny, we ended up with the idea that the right hand controls the position in 3d space and the left hand uses a pinching gesture to pick up objects. 

# The first pot-hole
The plan was initially to use ROS to control the arm and microROS to interface with our custom gripper however like many teams it turned out the ROS packages were not ready yet and could not communicate with the arm istelf so they were useless. This meant the only way to use the arm would be to use the application software provided and that could only move to coordinates in a pre-programmed manner. It did not inherently allow for dynamic updating of trajectories unless you could have a python script to write, compile and load a yaml file to a robot arm every 50ms which wasnt going to happen.

I left figuring out how to dynamically drive the arm to the IIT engineers which was a challenge easier said than done. Instead I worked on designing the gripper and figuring out how to communicate with it from a laptop.

# The Gripper
For the gripper i went with what the lab had available which was:
- an MG996R servo
- A load cell
- ESP32
- 3A 5V power supply 

The idea was that the MG996R would be responsible for the driving the gripper. It had enough torque for what we needed it to do but the fear was that repetively stalling the motor when the gripper is closed would cause it wear down quickly. We also wanted the gripper to be safe to use around people and the aim was to make it `actively compliant`

Compliancy is an important property that robots must possess if they want to be used around humans in dayto day life. As we know industrial robots in a factory workplace have barriers everywhere preventing people from coming too close to them. The reason is they are rigid in their path and can easily injure someone.

Having the robot be actibely compliant through means of current sensing, force sensing etc or `impedance control` makes them alot safer to use. 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="https://www.youtube.com/embed/XwiX2vv14Qs" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="https://www.youtube.com/embed/rFSXDaZQ4Qo" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

# The CAD
I used Fusion360 to CAD up the gripper design. As we were constrained by time, i decided not to go with the typical method of measuring very distance and modelling the gripper but instead i imported the servo and sketch out lines/rectanges to fit the shape of the servo. I gauged how much offset i should add and i must say this approach was risky given the printing itself would take a day so any re-designing would mean the gripper would not be finished in time.

There was an IGUS step file which i imported into Fusion 360 and then i modelled 4 parts to fit everything together:

# Pictures of the gripper
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/IGUS_gripper/IMG_2142.jpg" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/IGUS_gripper/IMG_2143.jpg" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/IGUS_gripper/IMG_2144.jpg" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
</div>
<!-- <swiper-container keyboard="true" navigation="true" pagination="true" pagination-clickable="true" pagination-dynamic-bullets="true" rewind="true">
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/IGUS_gripper/IMG_2142.jpg" class="img-fluid rounded z-depth-1" %}</swiper-slide>
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/IGUS_gripper/IMG_2143.jpg" class="img-fluid rounded z-depth-1" %}</swiper-slide>
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/IGUS_gripper/IMG_2144.jpg" class="img-fluid rounded z-depth-1" %}</swiper-slide>
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/IGUS_gripper/IMG_2153.jpg" class="img-fluid rounded z-depth-1" %}</swiper-slide>
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/IGUS_gripper/IMG_2154.jpg" class="img-fluid rounded z-depth-1" %}</swiper-slide>
  <swiper-slide>{% include figure.liquid loading="eager" path="assets/img/IGUS_gripper/IMG_2155.jpg" class="img-fluid rounded z-depth-1" %}</swiper-slide>
</swiper-container> -->