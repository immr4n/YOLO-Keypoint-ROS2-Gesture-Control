# Human Pose Estimation & Gesture Control with YOLOv8 + ROS2

> A robotics project built during my MSc in Artificial Intelligence — combining real-time computer vision with robot simulation in Gazebo/RViz2.

---

## What this project does

This package detects a person's body pose using a camera feed, estimates where each joint is in 3D space, and translates specific gestures into velocity commands that drive a robot. Everything runs inside a simulated environment using ROS2, Gazebo, and RViz2 — no physical robot needed.

I built this to understand how perception pipelines work in robotics: from raw image input, through a neural network, through filtering, all the way to an actuator command. That full pipeline is something I wanted to experience hands-on.

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| ROS2 (Humble) | Robot middleware & node communication |
| YOLOv8 Pose (`yolov8n-pose.pt`) | Real-time human keypoint detection |
| Kalman Filter | Smooth noisy 3D keypoint estimates |
| Gazebo + RViz2 | Simulation & visualization |
| Python 3.10 | All node logic |

---

## How it works

1. **YOLOv8 node** — detects 17 body keypoints from a camera image
2. **3D estimation node** — projects 2D keypoints into 3D using depth approximation
3. **Kalman filter node** — smooths the 3D positions to remove jitter
4. **Skeleton visualization** — publishes RViz markers so you can see the skeleton live
5. **Gesture-to-velocity node** — maps specific poses (e.g., arms raised) to `cmd_vel` commands

All nodes communicate over ROS2 topics. Launch the whole system with one command:

```bash
ros2 launch yolo_keypoint_sim full_system.launch.py
```

---

## Project Structure

```
ros2_ws/
└── src/
    └── yolo_keypoint_sim/
        ├── launch/
        │   └── full_system.launch.py
        └── yolo_keypoint_sim/
            ├── kalman_3d_node.py          # 3D smoothing
            ├── skeleton_marker_node.py    # RViz visualization
            ├── skeleton_bones_node.py     # Bone connections
            ├── keypoints_tf_broadcaster.py # TF frames
            ├── keypoints_to_markers.py    # Marker conversion
            └── gesture_to_cmdvel.py       # Robot control
```

---

## Setup & Running

```bash
# 1. Install Python dependencies
pip install -r ros2_ws/src/yolo_keypoint_sim/requirements.txt

# 2. Build the workspace
cd ros2_ws
colcon build
source install/setup.bash

# 3. Launch everything
ros2 launch yolo_keypoint_sim full_system.launch.py

# 4. Open RViz2 and add Marker + TF topics to visualize
rviz2
```

**Requirements:** ROS2 Humble/Iron, Python 3.10+, OpenCV, Ultralytics YOLOv8

---

## What I learned

- How to structure a multi-node ROS2 package from scratch
- Integrating a pre-trained YOLO model into a real-time ROS2 pipeline
- Applying Kalman filtering to reduce noise in pose estimation
- Publishing custom visualization markers in RViz2
- Translating high-level perception data into low-level robot commands

---

## Author

**Mohammed Imran Ibrahim**  
MSc Artificial Intelligence — Berlin School of Business and Innovation  
[LinkedIn](https://www.linkedin.com/in/imm4n) | mohammed.imran0306@gmail.com

---

*This project is for educational and research purposes.*
