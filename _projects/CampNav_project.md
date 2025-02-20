---
layout: page
title: Furuta Pendulum
description: LQR control of a Furuta Pendulum in Gazebo
img: assets/img/CampNav/campnav_thumbnail.png
importance: 2
category: School
---


# CampNav: Revolutionizing Campus Navigation with AR & Real-Time AI

CampNav is an innovative indoor navigation solution designed to transform the way students, faculty, and visitors navigate modern university campuses. By leveraging Augmented Reality (AR), real-time AI-powered voice assistance, and continuously updated campus data, CampNav replaces outdated maps and confusing signage with an intuitive, interactive experience.

---

## Table of Contents
- [Introduction](#introduction)
- [Project Overview](#project-overview)
- [The Challenge: Problem Statement](#the-challenge-problem-statement)
- [Our Solution: CampNav](#our-solution-campnav)
- [Technical Implementation](#technical-implementation)
  - [Technologies & Tools](#technologies--tools)
  - [System Architecture & Data Flow](#system-architecture--data-flow)
  - [Implementation Challenges](#implementation-challenges)
- [Technical Approach](#technical-approach)
  - [Augmented Reality Approach](#augmented-reality-approach)
  - [AI and Voice Integration](#ai-and-voice-integration)
  - [Path Planning & Routing](#path-planning--routing)
  - [Accessibility Considerations](#accessibility-considerations)
  - [User Experience & UI/UX](#user-experience--uiux)
- [Impact and Feasibility](#impact-and-feasibility)
  - [Expected Impact](#expected-impact)
  - [Scalability and Future Improvements](#scalability-and-future-improvements)
- [Business and Market Analysis](#business-and-market-analysis)
  - [Market Research](#market-research)
  - [Business Model & Revenue](#business-model--revenue)
  - [SWOT and PESTEL Analysis](#swot-and-pestel-analysis)
- [Demo and Presentation](#demo-and-presentation)
- [Team Reflections](#team-reflections)
- [Conclusion](#conclusion)
- [Additional Resources](#additional-resources)

---

## Introduction

Navigating sprawling university campuses can be overwhelming—even for those who are familiar with the layout. Traditional 2D maps and static signage fail to capture the dynamic nature of modern campuses. CampNav was born from the desire to simplify campus navigation by combining cutting-edge AR with real-time, AI-enhanced guidance. With CampNav, users simply scan a designated marker and receive an instantly rendered 3D AR map along with step-by-step voice instructions tailored to their needs.

---

## Project Overview

CampNav is a browser-based solution that does away with the need for downloads or complex installations. It delivers precise, real-time directions indoors by overlaying digital markers on a live camera feed. The system integrates a human-readable, easily updatable data repository so that campus administrators can seamlessly update information about room assignments, events, and facility changes. This real-time intelligence, combined with an AI conversational agent, ensures users receive accurate guidance regardless of changes in the campus environment.

---

## The Challenge: Problem Statement

University campuses are intricate networks of similar-looking buildings and hidden corridors. Key issues include:

- **Inefficient Navigation:** Traditional 2D maps are not optimized for indoor environments, leading to significant delays.
- **Time Constraints:** Students often have only 5–10 minutes between classes, and any navigation inefficiency can cause delays.
- **Accessibility Limitations:** Existing navigation tools rarely accommodate the needs of users with disabilities, such as wheelchair users or those with visual impairments.
- **Dynamic Campus Environments:** Rapid changes in room assignments, temporary closures, or new construction require a navigation system that is constantly updated.

These challenges emphasize the need for a dynamic, user-centric navigation solution tailored to the complex environments found on modern campuses.

---

## Our Solution: CampNav

CampNav addresses these challenges head-on by combining:

- **Web-Based AR Navigation:** Instant access via any web browser—no app installation required.
- **Real-Time AR Overlays:** Digital markers and directional arrows are superimposed onto the user’s live camera view for clear, intuitive navigation.
- **Intelligent Voice Assistance:** An AI agent processes natural language queries (e.g., “Where is Lecture Hall B?”) and provides immediate, context-sensitive directions.
- **Dynamic Data Integration:** A continuously updated databank ensures that navigation instructions reflect the latest campus changes, from room reassignments to event notifications.

By integrating these features, CampNav transforms the campus experience—making navigation more efficient, accessible, and enjoyable.

---

## Technical Implementation

### Technologies & Tools

CampNav’s robust backend and front-end technologies include:

- **Voice Recognition:** Powered by LiveKit for accurate natural language command processing.
- **Real-Time Database:** Firebase is used to store and synchronize up-to-date campus data.
- **Web Frameworks:** 
  - **FastAPI & Flask:** For high-performance web APIs and lightweight Python-based API services.
  - **Next.js & React:** For responsive server-side rendering and a dynamic user interface.
- **Augmented Reality:** 
  - **Zappar & WebXR:** To facilitate AR integration that works seamlessly across devices.
  - **Three.js:** For rendering engaging 3D graphics within the browser.
- **Real-Time Communication:** WebRTC ensures low-latency voice and video streaming between users and the system.

### System Architecture & Data Flow

The CampNav system is designed as an interconnected pipeline:

1. **User Input:** A user interacts via voice commands captured on a mobile device.
2. **Route Calculation:** The backend fetches real-time campus data and computes the optimal route using algorithms (including an A* path planning algorithm for multi-level navigation).
3. **AR Rendering:** Once the route is calculated, digital overlays and directional markers are rendered on the user’s live camera feed.
4. **Continuous Updates:** The system maintains up-to-date campus information via Firebase and a CDN that delivers 3D assets and images quickly.

### Implementation Challenges

Several challenges were tackled during development:
- **Real-Time Data Handling:** Ensuring minimal latency during data retrieval and route recalculations.
- **AR Stability:** Maintaining consistent AR performance across diverse device types and lighting conditions through device-specific calibrations.
- **Backend Optimizations:** Overcoming latency issues and ensuring that route updates are delivered almost instantaneously.

---

## Technical Approach

### Augmented Reality Approach

CampNav employs a **marker-based AR system** for broad device compatibility. Although more advanced spatial AR (using SLAM) exists, marker-based AR offers:
- **Immediate Visual Feedback:** Ensures low latency and consistent performance.
- **Wide Accessibility:** Works efficiently on nearly all mobile devices with minimal setup.

### AI and Voice Integration

Key aspects of the AI component include:
- **Natural Language Processing:** Leveraging large language models to translate spoken commands into actionable navigation requests.
- **Contextual Awareness:** Integration with Firebase ensures that the AI agent provides context-sensitive responses (e.g., current operating hours, room capacities).
- **Real-Time Communication:** A WebRTC-inspired channel supports fluid, low-latency conversation between the user and the system.

### Path Planning & Routing

For complex indoor environments:
- **Graph-Based Mapping:** Each floor is modeled as a node graph, with nodes representing key locations (e.g., stairs, elevators, corridors).
- **A* Algorithm:** Efficiently computes the shortest and safest route across multi-level buildings.
- **Adaptive Routing:** The system dynamically recalculates paths in response to obstacles such as temporary closures or congestion.

### Accessibility Considerations

Inclusivity is a core design principle:
- **Screen Reader Support:** The interface is optimized for compatibility with assistive technologies.
- **Voice-Only Navigation:** Enables hands-free operation for users with limited mobility.
- **Accessible Marker Placement:** Physical markers are strategically installed at accessible heights and locations.

### User Experience & UI/UX

To ensure an optimal user experience:
- **Intuitive AR Overlays:** Directions and cues are unobtrusively integrated into the real-world view.
- **Minimalist Design:** A clean, clutter-free interface ensures that only the essential information is displayed.
- **Administrative Tools:** A dedicated console allows campus administrators to update navigation data, manage markers, and schedule notifications with ease.

---

## Impact and Feasibility

### Expected Impact

CampNav’s innovative solution is expected to:
- **Save Time:** Streamline campus navigation and reduce delays between classes.
- **Reduce Stress:** Provide a clear, user-friendly interface that minimizes confusion.
- **Enhance Inclusivity:** Cater to a diverse range of users, including those with disabilities, by offering tailored navigation assistance.
- **Foster Community Engagement:** Enable users to contribute tips and insights, creating a collaborative campus environment.

### Scalability and Future Improvements

- **Scalability:** The web-based architecture and centralized administrative console make CampNav easily deployable to various campuses—and even adaptable to venues such as hospitals, malls, or corporate complexes.
- **Future Enhancements:** Plans include exploring dedicated VPS hosting, containerization, and microservices architectures to further reduce latency and improve system performance.

---

## Business and Market Analysis

### Market Research

Research indicates that:
- **Current Gaps:** Traditional navigation tools (e.g., paper maps, static digital maps) are insufficient for the dynamic and expansive nature of modern campuses.
- **User Demand:** There is a growing need for interactive, real-time navigation solutions among students and visitors who rely heavily on smartphones.

### Business Model & Revenue

CampNav’s revenue model is based on:
- **Institutional Subscriptions:** Universities subscribe to provide the service campus-wide.
- **Partnerships:** Strategic collaborations with campus vendors and local businesses, opening up additional revenue streams through targeted promotions.
- **Flexible Pricing:** Tariffs can be customized based on beacon installations and user volumes, ensuring cost-effectiveness and scalability.

### SWOT and PESTEL Analysis

- **SWOT Analysis:**
  - **Strengths:** Innovative integration of AR, AI, and real-time data; browser-based accessibility; inclusive design.
  - **Weaknesses:** Dependence on physical markers and initial setup costs.
  - **Opportunities:** Expansion into other large venues (hospitals, malls) and the growing market for smart campus solutions.
  - **Threats:** Rapid technological changes and competition from emerging navigation platforms.

- **PESTEL Analysis:**
  - **Political:** Supportive digital education policies and smart campus initiatives.
  - **Economic:** Increased investment in educational technology and campus innovation.
  - **Social:** Rising expectations for high-quality, user-friendly digital services.
  - **Technological:** Advancements in AR, AI, and real-time data processing.
  - **Environmental:** Digital solutions reduce paper waste and physical signage.
  - **Legal:** Robust data privacy and security measures compliant with regulations (e.g., GDPR).

---

## Demo and Presentation

CampNav’s demo showcases the practical application of our technology:
- **Demo Snapshots:** Images and video clips highlight indoor and outdoor AR navigation in various campus settings.
- **Demo Links:** A [demo video](https://youtube.com/shorts/APUYnpqRj5w?si=XB9B970USzQROsxq) provides a quick look at the system in action.

---

## Team Reflections

Our journey building CampNav was both challenging and rewarding. Key takeaways include:

- **Iterative Prototyping:** Constant user feedback helped refine both the UI/UX and the underlying technical architecture.
- **Collaboration:** Cross-disciplinary teamwork was critical in overcoming technical challenges such as latency issues and AR calibration.
- **Learning and Adaptation:** The project underscored the importance of agile development and rapid iteration in creating innovative technology solutions.

---

## Conclusion

CampNav stands as a forward-thinking solution for the navigational challenges of modern campuses. By merging AR, AI-powered voice assistance, and real-time data integration, our project not only streamlines the navigation process but also enhances overall accessibility and user satisfaction. We are excited about the potential of CampNav to redefine campus experiences and look forward to its continued evolution and widespread adoption.

---

## Additional Resources

- **Full Technical Report:** Detailed insights into system architecture, technical challenges, and future enhancements.
- **Pitch Deck Overview:** A concise summary of our project’s vision and market potential.
- **Contact Information:** For further inquiries or collaboration opportunities, please reach out to our team via the contact details provided in our official pitch documents.

---

*CampNav – Your AI Powered Campus Guide. Just scan, ask, and navigate with ease!*
