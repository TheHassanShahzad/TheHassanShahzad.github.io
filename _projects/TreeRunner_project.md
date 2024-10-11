---
layout: page
title: TreeRunner
description: A Tree Climbing robot capable of Branch Avoidance
img: assets/img/TreeRunner/best.jpg
importance: 3
category: work
---

At the end of my first year at Robotics and AI @ UCL we had a term 3 project challenge which was to make a tree climbing robot. It was tasked to go up, deploy a sensor and come back down. The trees to climb varied in difficiulty; the simplest being a pipe of fixed diameter and the hardest was a wooden pole with varying diameter and branches.

Each group had total creative freedom over their designs and quickly the choices diverged into 2 clear options. One was a wheeled design capable of ascending and descending the simple trees quickly but intentinally fail at the branches. The second choice, which our group went with, was a tree composed of 2 grippers that could retract and contract using some linear actuator based system. To avoid the branches, a unique rotating mechanism was to be employed. More on this later

# The Design 

The robot was composed of 2 grippers and a linear actuator in between. Another servo was used to rotate the top servo allowing it to rotate around the tree and climb independently.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/TreeRunner/top_gripper.png" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/TreeRunner/bottom_gripper.png" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
</div>

The grippers each used powerful `MG996R` servos providing the grippers high torque. This was crucial because without good grip, any kind of climbing would fail let alone branch avoidance. Without strong grip we would have had to work on reducing the weight as much as possible which is an interesting engineering problem to tackle in its own right but i was far more interested in implementing the mechanical complexity of the tree climbing robot. 

Each servo had a gear attached to it and another gear with a 1:1 ratio. This meant that the gripper could open in a symmetric motion and not rely on 2 servos. At first i was worried if the gear would be strong enough but they definitely were. For added strength we just increased the height of the gearts which also increased the surface area of the contact points spreading the load. Bearings were used on the child gear to reduce contact friction and let the gears spin more easily.


Each gripper was attached to the servo using a metal circular servo horn and thread inserts pushed in with a soldering iron. This ensured the grippers were attached firmly to the servo and would not slip when the servos were stalling. Stalling the servos turned out not to be a good idea in the long run which I will discuss more later on.


<div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/TreeRunner/tri_prong.png" class="img-fluid rounded z-depth-1 zoomable=true" %}
</div>


Each gripper had 3 fingers which intersected to handle pipes of varying diamter and increase the contact surface area between he gripper and the tree. Becuase printing the piece with 2 prongs would require a lot of supprt and if it needed some changing in the futre, we would have to re print the entire part, I decided to split it into 3 section. The middle section securing the top and bottom prong together. Thread inserts were again used to fasten everything together. Sillicone rubber strips were used that we laser cut and then super glued to the grippers once our design was complete.

The linear actuator was composed of a 12V DC geared motor with an encoder and a micro switch to make sure the gripper never exceeded its range of motion. Before the robot began climbing, it ran its homing sequence in which it moved the motor until the switch was pressed then it backed down a certain number of encoder counts calculated using the length of the rod, the encoder count resolution and the diameter of the thereaded rod. The microswitch was attached to the moving base of the bottom gripper and was hit when it came into contact with the servo responsible for rotation

The rotating mechanism was on the biggest CAD design problems faced because i didnt know where to begin. After a few days of thinking countless possibilities for how the robot can avoid branches, i had an idea based on how simple gears work. We take one small gear and a larger gear witha  higher tooth count and cut the larger gear into a third and strip away everything that isnt the gear teeth. What were left with is a gear system with a constrained range of motion but its fine because now the larger gear can "wrap" around a circle or in other words a tree. Such a system meant I could have a servo that allowed the robot to take multiple steps around a tree in order to avoid a branch. After a few revision of CAD for the model, i had it finished. I needed several tests because there were various weak points in the system and areas of potential friction. The revolving mechanism had to be strong because many times on a robots climb all the weight is handing by the revolving mechanism when the top gripper is closed and bottom open. 

# The Electronics
### Here is the list of electronic components used in the board:
- Seeeduino Xiao
- L293D
- VL53L0X ToF sensor
- 5A 5V step down converter
- PCB Screw terminals

The `Seeeduino xiao` was used for its small compact design and easy programming with the Arduino IDE. Although it had just enough GPIO pins, i would say it was still an overkill choice but since the lab was giving them i thought its best to use them for the project. 

The L293D is a dual channel H-bridge motor driver but since we have 1 DC motor with current draw requirements higher than what the chip itself can give, I thought why not tie the channels together and double the max current draw. This seemed to work without any problems. If you are wondering why not use a diffrent driver with higher current capabillities, it was what the lab had that was light weight. Other options was a bulky L298N which the robot had no need for and no space.

I used a 5A 5V power supply along with a 12V one to power the DC motor and servos. The power supply for the servos was crucial else they would not have anywhere as much torque as required and likely burn out any low current step down regulators.

The PCB screw terminals made wiring the encoder, motor, power supply and I2C compoennst alot easier. I previously tried some actual JST clip cables but that causes data loss in the I2C data making it unusable for some reason. 




