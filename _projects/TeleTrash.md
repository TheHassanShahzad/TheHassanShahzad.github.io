---
layout: page
title: Teleoperated Trash Picking Robot 
description: A low latency robot that is driven with akeyboard but controlled with HAND gestures using google mediapipe
img: assets/img/TeleTrash/main.jpg
importance: 2
category: Completed
---

# SmartGreen2: A Teleoperated Trash Picking Robot

*Built in 2.5 days at the Gran Canaria Smart Green Makeathon*

## The Challenge: Beyond Autonomous AI

During my second visit to the Gran Canaria Smart Green Makeathon, I witnessed team after team attempting to build autonomous trash-picking robots using AI and computer vision. While technically impressive, I knew these approaches faced fundamental limitations:

- **Narrow applicability**: AI models trained for specific environments don't generalize well
- **Complexity overhead**: Object detection, path planning, and manipulation in unstructured environments
- **Real-world reliability**: Autonomous systems struggle with edge cases and unexpected scenarios

Instead of following the crowd, I took a radically different approach: **What if we kept the human in the loop but made the interface intuitive enough for real-time control?**

## The Innovation: Hand-Tracking Teleoperation

The core insight was simple yet powerful: humans are excellent at identifying trash and making manipulation decisions, but traditional interfaces (keyboards, joysticks) are too slow for fluid robot arm control. My solution bridged this gap using computer vision to track hand gestures and translate them into robot movements in real-time.

### The Technical Architecture

The system consists of two main components working in perfect harmony:

#### 1. Computer Vision Hand Tracking (Python)
- **MediaPipe Integration**: Leveraged Google's MediaPipe for robust hand landmark detection
- **Gesture Mapping**: 
  - Wrist X/Y position → Robot arm base rotation and elbow angle
  - Thumb-index finger pinch angle → Gripper open/close
- **Real-time Performance**: 20Hz update rate for responsive control
- **UDP Communication**: Low-latency JSON packets sent over WiFi

#### 2. ESP32 Robot Controller (Arduino)
- **Dual UDP Ports**: Separate communication channels for arm control (12345) and base movement (12346)
- **Servo Control**: Precise angle control for 3-DOF arm (base, elbow, gripper)
- **Motor Driving**: L298N motor driver for differential drive base
- **JSON Parsing**: Real-time command interpretation with ArduinoJson library

## System Architecture

```
[Laptop Camera] → [Hand Tracking] → [UDP Packets] → [ESP32] → [Robot Hardware]
     ↓                    ↓              ↓           ↓
  MediaPipe         Gesture Mapping   WiFi Comm   Servo Control
   - Wrist XY       - Base Rotation   Port 12345   - Base Servo
   - Pinch Angle    - Elbow Angle     Port 12346   - Elbow Servo  
                    - Gripper State               - Gripper Servo
                                                  - Drive Motors
```

## The Development Marathon

This project was completed in an intense **2.5-day sprint** with an average of only **3 hours of sleep per night**. The development process was a rollercoaster:

### Day 1: Foundation and Fumbles
- **Morning**: Started with servo calibration and basic UDP communication
- **Afternoon**: Implemented hand tracking with MediaPipe
- **Evening**: First successful hand-to-servo mapping
- **Late Night**: Debugging coordinate transformations and angle calculations

### Day 2: Integration Hell
- **Early Morning**: Motor control implementation for the base
- **Afternoon**: The frame disaster - scrapped the provided chassis for custom build
- **Evening**: 3D printing custom joints and links
- **Night**: Tupperware electronics housing (necessity is the mother of invention!)

### Day 3: Race Against Time
- **Morning**: Final integration and calibration
- **Afternoon**: Extensive testing and parameter tuning
- **Final 30 Minutes**: Last-minute bug fixes before the presentation

## Technical Deep Dive

### Hand Tracking Mathematics

The system uses vector mathematics to calculate the pinch gesture:

```python
# Calculate vectors from wrist to fingertips
vec_thumb = np.array([thumb_x - wrist_x, thumb_y - wrist_y])
vec_index = np.array([index_x - wrist_x, index_y - wrist_y])

# Calculate angle between vectors
dot_product = np.dot(vec_thumb, vec_index)
magnitude = np.linalg.norm(vec_thumb) * np.linalg.norm(vec_index)
pinch_angle = np.degrees(np.arccos(dot_product / magnitude))
```

### Real-Time Mapping System

The elegant mapping between human gestures and robot movements:

```python
# Map hand position to robot coordinates
base_angle = np.interp(wrist_x, [0, 1], [180, 0])    # Horizontal movement
elbow_angle = np.interp(wrist_y, [0, 1], [0, 180])   # Vertical movement
gripper_angle = np.interp(pinch_angle, [5, 50], [25, 150])  # Pinch to grip
```

### ESP32 Dual Communication

The robot listens on two UDP ports simultaneously:

- **Port 12345**: Arm control (base, elbow, gripper angles)
- **Port 12346**: Base movement (linear and angular velocity)

This separation allowed independent control of the manipulator and mobile base, crucial for effective teleoperation.

## Hardware Implementation

### The Custom Frame Decision
One of my biggest challenges (and potential mistakes) was deciding to build a custom frame from scratch rather than using the provided chassis. While this consumed precious time, it ultimately provided:

- Better range of motion for the arm
- Optimal sensor placement
- Custom mounting points for 3D printed components

### Ingenious Solutions
- **Tupperware Electronics Housing**: Waterproof, accessible, and surprisingly effective
- **Portable Charger Power**: Mobile and sufficient for the servo loads
- **3D Printed Joints**: Custom mechanical interfaces printed overnight

## The Final Showcase

With just **30 minutes** before presenting to a room of hundreds of talented German engineers, everything finally clicked. The system demonstrated:

- **Intuitive Control**: Operators could pick up objects within minutes of instruction
- **Low Latency**: Real-time response that felt natural and responsive
- **Robust Operation**: Stable performance despite the quick development timeline
- **Practical Utility**: Actually capable of picking up various types of trash

## Key Innovations

### 1. Gesture-Based Manipulation
Unlike traditional joystick or keyboard control, hand gestures provide an intuitive interface that leverages natural human motor skills.

### 2. Dual-Channel Communication
Separating arm and base control allowed operators to move and manipulate simultaneously, significantly improving efficiency.

### 3. Real-Time Parameter Mapping
The mathematical transformation from gesture space to joint space was carefully calibrated for natural feel and full range utilization.

### 4. Lightweight Architecture
The entire system ran on a standard laptop and ESP32, making it portable and cost-effective.

## Lessons Learned

### Technical Insights
- **MediaPipe is incredibly robust** for real-time hand tracking
- **UDP communication** provides the low latency essential for teleoperation
- **ESP32 dual-core architecture** handles multiple communication streams effectively
- **Custom hardware** can be worth the time investment for specific use cases

### Project Management
- **Sleep is important** - those 3-hour nights caught up with me
- **Incremental testing** saved the project multiple times
- **Simple solutions often work better** than complex autonomous systems
- **Deadline pressure** can drive incredible innovation

### Design Philosophy
- **Human-machine collaboration** often outperforms full automation
- **Interface design** is as important as the underlying technology
- **Practical constraints** drive creative solutions

## Future Improvements

While the project was successful within the makeathon constraints, several enhancements could make it even more effective:

- **Force feedback** through haptic gloves
- **Stereo vision** for better depth perception
- **Machine learning** for gesture prediction and smoothing
- **Multiple camera angles** for comprehensive workspace coverage
- **Wireless charging station** for extended operation

## Conclusion

This project proved that sometimes the best solution isn't the most complex one. By keeping humans in the control loop while providing an intuitive interface, we created a trash-picking robot that was both immediately usable and highly effective.

The combination of computer vision, real-time communication, and thoughtful human-machine interface design resulted in a system that worked reliably under pressure and demonstrated the power of collaborative robotics over pure automation.

Most importantly, it showed that with creativity, determination, and very little sleep, incredible things can be built in just 2.5 days.

---

## Technical Files Overview

### Arduino Firmware (`/firmware/`)
- `complete.ino`: Main ESP32 firmware with dual UDP communication
- `servo_calibrate.ino`: Servo calibration utilities
- `1motortest.ino` & `dualmotor.ino`: Motor control development
- `udp_getter_basic.ino`: Basic UDP communication testing

### Python Control System (`/python/hassan/`)
- `complete.py`: Main hand tracking and robot control system
- `drive.py`: Keyboard-based robot base control
- `pinch_angle.py`: Pinch gesture detection development
- `wrist_units.py`: Wrist position mapping utilities
- `simple_UDP.py` & `udp_sender.py`: Communication testing tools

*Built with passion, caffeine, and minimal sleep at the Gran Canaria Smart Green Makeathon 2024*