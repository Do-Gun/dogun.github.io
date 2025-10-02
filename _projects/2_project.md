---
layout: page
title: Autonomous Driving Competition
description: "The 4th International University Students Autonomous EV Driving Competition<br>Chairman's Award Winner!<br><br>International e-Mobility Expo, Global EV Association Network<br>Team: Gungyoung Ko, Dogun Kim, Taehee Lee, Sungmin Yoon, Subin Lee"
img: assets/img/ad/1.gif
importance: 2
category: work
---

## 0. Overview
<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        <img src="{{ 'assets/img/ad/1.gif' | relative_url }}" class="img-fluid rounded z-depth-1" style="height: 400px; width: 100%; object-fit: contain;">
        <div class="caption"><strong>Autonomous Driving Demo</strong></div>
    </div>  
    <div class="col-sm-6 mt-3 mt-md-0">
        <img src="{{ 'assets/img/ad/1.jpg' | relative_url }}" class="img-fluid rounded z-depth-1" style="height: 400px; width: 100%; object-fit: contain;">
        <div class="caption"><strong>Autonomous Vehicle System</strong></div>
    </div>
</div>

This project was conducted for The 4th International University Students Autonomous EV Driving Competition. We developed an autonomous driving system utilizing various sensors including Camera, LiDAR, and Odometry, achieving the **Chairman's Award**.

---

## 1. Competition Introduction

### Competition

<div class="row mt-3">
    <div class="col-sm-6 mt-3 mt-md-0 mx-auto">
        {% include figure.liquid path="assets/img/ad/poster.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The 4th International University Students Autonomous EV Driving Competition Official Poster
</div>

The 4th International University Students Autonomous EV Driving Competition is an international autonomous electric vehicle competition hosted by Global EV Association Network at the International e-Mobility Expo.

### Track Sections and Constraints

<div class="row mt-3">
    <div class="col-sm-8 mt-3 mt-md-0 mx-auto">
        {% include figure.liquid path="assets/img/ad/3.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Competition Track Configuration
</div>

Each section requires the use of specific sensors, with strict disqualification rules for non-compliance:

- **Section A (Lane Following)**: Camera sensor only
- **Section B (Obstacle Avoidance)**: LiDAR sensor only  
- **Section C (Localization & Path Planning)**: Odometry sensor only

---

## 2. Hardware Configuration

<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        <img src="{{ 'assets/img/ad/4.png' | relative_url }}" class="img-fluid rounded z-depth-1" style="height: 300px; width: 100%; object-fit: contain;">
        <div class="caption"><strong>Autonomous Vehicle</strong></div>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        <img src="{{ 'assets/img/ad/5.png' | relative_url }}" class="img-fluid rounded z-depth-1" style="height: 300px; width: 100%; object-fit: contain;">
        <div class="caption"><strong>Jetson Nano System</strong></div>
    </div>
</div>

**Key Components:**
- **Jetson Nano**: Main computing board
- **2D LiDAR**: 360-degree distance measurement
- **Camera**: Lane and environment recognition
- **IMU**: Vehicle pose measurement
- **Motor Controller**: Steering and speed control

All sensors are integrated through ROS framework, enabling real-time data processing in a unified system.

---

## 3. Method

### Section A: Lane Following

**1) Camera Calibration**

<div class="row mt-3">
    <div class="col-sm-6 mt-3 mt-md-0 mx-auto">
        {% include figure.liquid path="assets/img/ad/30.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Checkerboard-based Camera Calibration using Zhang's Method
</div>

Zhang's Method was employed to accurately calculate camera intrinsic parameters for distortion correction.

**2) Lane Detection Algorithm**

<div class="row">
    <div class="col-sm-3 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/ad/6.png" class="img-fluid rounded z-depth-1" %}
        <div class="caption"><strong>Bird's Eye View</strong></div>
    </div>
    <div class="col-sm-3 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/ad/7.png" class="img-fluid rounded z-depth-1" %}
        <div class="caption"><strong>HSV + Filter</strong></div>
    </div>
    <div class="col-sm-3 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/ad/8.png" class="img-fluid rounded z-depth-1" %}
        <div class="caption"><strong>Sliding Window</strong></div>
    </div>
    <div class="col-sm-3 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/ad/9.png" class="img-fluid rounded z-depth-1" %}
        <div class="caption"><strong>Result</strong></div>
    </div>
</div>

- Bird's Eye View transformation for vanishing point distortion correction
- HSV color space conversion and Sobel filtering for robust lane detection
- Sliding window technique for reliable lane tracking
- PID control for steering angle determination based on lateral error

<div class="row mt-3">
    <div class="col-sm-6 mt-3 mt-md-0 mx-auto">
        <img src="{{ 'assets/img/ad/40.gif' | relative_url }}" class="img-fluid rounded z-depth-1" style="width: 100%; height: auto;">
    </div>
</div>
<div class="caption">
    Section A Lane Following Final Result
</div>

### Section B: Obstacle Avoidance

<div class="row">
    <div class="col-sm-3 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/ad/LiDAR.png" class="img-fluid rounded z-depth-1" %}
        <div class="caption"><strong>RPLIDAR A1</strong></div>
    </div>
    <div class="col-sm-3 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/ad/11.png" class="img-fluid rounded z-depth-1" %}
        <div class="caption"><strong>Practice Environment</strong></div>
    </div>
    <div class="col-sm-3 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/ad/12.png" class="img-fluid rounded z-depth-1" %}
        <div class="caption"><strong>Gmapping SLAM</strong></div>
    </div>
    <div class="col-sm-3 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/ad/13.png" class="img-fluid rounded z-depth-1" %}
        <div class="caption"><strong>Navigation</strong></div>
    </div>
</div>

- Real-time obstacle detection and environment perception using RPLIDAR A1
- Simultaneous mapping and localization through Gmapping SLAM
- Optimal path planning utilizing ROS move_base package
- Dynamic obstacle avoidance with safety distance maintenance

<div class="row mt-3">
    <div class="col-sm-6 mt-3 mt-md-0 mx-auto">
        <img src="{{ 'assets/img/ad/60.gif' | relative_url }}" class="img-fluid rounded z-depth-1" style="width: 100%; height: auto;">
    </div>
</div>
<div class="caption">
    Section B Obstacle Avoidance Final Result
</div>

### Section C: Odometry-based Localization

**Technical Approach:**
- Precision odometry implementation fusing IMU and encoder data
- Accurate localization for goal point navigation
- Low-pass filtering for sensor noise reduction
- Real-time pose correction and path following algorithms

---

## 4. Results

<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        <img src="{{ 'assets/img/ad/1.gif' | relative_url }}" class="img-fluid rounded z-depth-1" style="height: 400px; width: 100%; object-fit: contain;">
        <div class="caption"><strong>Autonomous Driving Demo</strong></div>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        <img src="{{ 'assets/img/ad/15.png' | relative_url }}" class="img-fluid rounded z-depth-1" style="height: 400px; width: 100%; object-fit: contain;">
        <div class="caption"><strong>Chairman's Award</strong></div>
    </div>
</div>

**Competition Achievements:**
- **Chairman's Award** recipient
- Successful autonomous navigation in all sections using designated sensors only
- Real-time sensor data processing through ROS-based integrated system

Outstanding results were achieved through systematic system design while adhering to section-specific constraints.

---

## 5. Limitations and Future Work

**Limitations:**
- Sensitivity of lane detection algorithm to lighting variations
- Accuracy limitations due to sensor noise

**Future Improvements:**
- Upgrade to high-performance hardware platforms
- Implementation of deep learning-based robust perception systems
- Enhanced accuracy through multi-sensor fusion techniques

---

<div class="row mt-5">
    <div class="col-sm-12 text-center">
        <h4>Team GOSARF</h4>
        <p>Taehee Lee (Leader) · Gungyoung Ko · Dogun Kim · Sungmin Yoon · Subin Lee</p>
        <p><em>The 4th International University Students Autonomous EV Driving Competition</em></p>
        <p><em>Chairman's Award Winner</em></p>
    </div>
</div>
