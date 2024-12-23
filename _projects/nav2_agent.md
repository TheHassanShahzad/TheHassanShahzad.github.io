---
layout: page
title: LLM Agent control of a Autonomous Robot
description: A Generalized software for LLM's to control Autonomous Robots using Langchain and webRTC based Realtime Communication
img: assets/img/nav2_agent/house.png
importance: 2
category: School
---

## Overview

This project explores using advanced Large Language Models (LLMs) for controlling a ROS2-based robot (A24). By integrating LangChain for text-based control and LiveKit WebRTC for speech-based interaction, we demonstrate how LLMs can bridge the gap between humans and robots. The project also leverages OpenAI’s API for real-time decision-making, enabling natural language queries to translate into robotic actions. Be sure to check out the associated publication on my website for this project.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nav2_agent/house.png" title="furuta upright" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
</div>

## Objectives

- Use LangChain to allow **text-to-text** control of the A24 robot.  
- Use LiveKit WebRTC to enable **speech-to-speech** interaction with the robot.  
- Enhance robot capabilities with ROS2, Nav2, and custom tools for robust navigation and control.  
- Demonstrate tool chaining with LangChain for complex queries.

## Key Features

1. **LangChain Integration**  
   - Added tools to LangChain, allowing the robot to:  
     - Navigate to specific coordinates.  
     - Capture and describe images using OpenAI's API.  
     - Determine distances from depth cameras.  
     - Echo ROS2 topics for debugging and odometry readings.  
   - Tool chaining enables handling multi-step queries, e.g., determining the robot's location and time.  

2. **LiveKit WebRTC**  
   - Developed a speech-to-speech interaction system using LiveKit's real-time communication platform.  
   - Integrated a pipeline: **Speech-to-Text (STT)** → **LLM Query** → **Text-to-Speech (TTS)**.  
   - Allowed seamless interaction through a smartphone or other devices.  

3. **ROS2 Integration**  
   - Controlled robot navigation using the **Nav2 stack** and SLAM Toolbox.  
   - Used a Gazebo simulation to validate the system's performance.  
   - Enhanced localization with Adaptive Monte Carlo Localization (AMCL).  

4. **Simulation Setup**  
   - Simulated the A24 robot in a Gazebo environment representing a home.  
   - Generated maps using `slam_toolbox` and navigated autonomously within the environment.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nav2_agent/a24_tf.png" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nav2_agent/openbase2_tf.png" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nav2_agent/world.png" title="furuta upright" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
</div>

## Results

### LangChain
- Successfully executed commands like:
  - "Move forward."
  - "Where are you?" (using `/odom` topic for coordinates).
- Demonstrated robust tool chaining to solve multi-step queries.

### LiveKit
- Enabled speech-to-speech queries such as:
  - "Make the robot move forward."
  - "Stop the robot."
- Highlighted limitations in tool chaining compared to LangChain.

### ROS2 & Navigation
- Generated accurate maps using SLAM Toolbox and navigated autonomously.  
- Tested navigation with waypoints, achieving high accuracy in localization.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nav2_agent/world_top.png" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nav2_agent/world_map.png" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
</div>

## Tools and Libraries Used

- **LangChain**: LLM framework for integrating tools.  
- **LiveKit WebRTC**: Real-time speech-to-speech interaction.  
- **OpenAI API**: For natural language processing and image descriptions.  
- **ROS2**: Robot Operating System for middleware and control.  
- **Nav2**: Navigation stack for autonomous movement.  
- **slam_toolbox**: For mapping and localization.  
- **Gazebo**: Robot simulation environment.  

## Repository Structure

- **agent_code/**:  
  - Contains LangChain and LiveKit agents.  
  - Includes tools for image recognition, navigation, and querying ROS2 topics.  

- **launch/**:  
  - Launch files for AMCL, SLAM, and navigation.  

- **maps/**:  
  - Includes pre-generated maps for testing navigation.  

- **urdf/**:  
  - Unified Robot Description Format files for the A24 robot.  

## Challenges and Learnings

- **Tool Chaining**:  
  LangChain demonstrated better tool chaining capabilities than LiveKit, allowing multi-step task execution.  

- **Sim2Real**:  
  Testing in Gazebo provided insights into real-world deployment challenges, especially in localization and navigation.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nav2_agent/describe_gym.png" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nav2_agent/describe_image.png" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
</div>
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nav2_agent/move_to_point.png" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nav2_agent/goto_goal.png" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
</div>

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nav2_agent/slam_tf.png" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nav2_agent/2d_3d.png" class="img-fluid rounded z-depth-1 zoomable=true" %}
    </div>
</div>

## Future Work

- Integrate the system with a physical A24 robot for real-world testing.  
- Expand the toolset for LangChain and LiveKit to handle more complex tasks.  
- Improve LiveKit's ability to chain tools for multi-step queries.

## Conclusion

This project showcases the power of LLMs in human-robot interaction. By combining LangChain’s tool integration with LiveKit’s real-time communication capabilities, we demonstrate a scalable, low-computation solution for controlling autonomous robots using natural language.
