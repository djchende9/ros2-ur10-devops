# ROS2 UR10 DevOps Framework

## Overview
This repository contains a ROS2-based simulation and control setup for a UR10 robotic manipulator using Gazebo and ros2_control.

## Features
- ROS2 Humble + Gazebo integration
- UR10 simulation (URDF + xacro)
- ros2_control with trajectory execution
- Ready for MATLAB integration

## Setup

Install dependencies:

```bash
sudo apt update
sudo apt install ros-humble-desktop
sudo apt install ros-humble-gazebo-ros-pkgs
```
## Run Simulation
```bash
ros2 launch gazebo_ros gazebo.launch.py gui:=false
```

## Run UR10 Control
```bash
ros2 launch ur_robot_driver ur_control.launch.py \
ur_type:=ur10 \
robot_ip:=192.168.0.1 \
use_fake_hardware:=true
```

## Control Interface
Publish trajectories to:
```bash
/scaled_joint_trajectory_controller/joint_trajectory
````
Subscribe to:
```bash
/joint_states
```

## MATLAB Integration

This framework supports external control via MATLAB using the ROS2 Toolbox.  
The integration follows a publish–subscribe model over standard ROS2 topics.

---

### 1. Connect MATLAB to ROS2

Start MATLAB and initialize a ROS2 node:

```matlab
node = ros2node("/matlab_node");
```
Verify available topics:

```matlab
ros2 topic list
```

###2. Subscribe to Joint States

The robot state is published on:
/joint_states

Create a subscriber:
```matlab
sub = ros2subscriber(node, "/joint_states");
msg = receive(sub, 10);
disp(msg.Position);
```

This provides real-time joint positions of the UR10 robot.
###3. Publish Trajectory Commands

The controller accepts trajectory commands on:
/scaled_joint_trajectory_controller/joint_trajectory

Create a publisher:
```matlab
pub = ros2publisher(node, ...
"/scaled_joint_trajectory_controller/joint_trajectory", ...
"trajectory_msgs/JointTrajectory");
```

Construct a trajectory message:
```matlab
msg = ros2message(pub);

msg.joint_names = {
    'shoulder_pan_joint'
    'shoulder_lift_joint'
    'elbow_joint'
    'wrist_1_joint'
    'wrist_2_joint'
    'wrist_3_joint'
};

pt = ros2message("trajectory_msgs/JointTrajectoryPoint");
pt.positions = [0, -1.57, 1.57, 0, 0, 0];
pt.time_from_start.sec = 3;

msg.points = pt;

send(pub, msg);
```

### 4. Execution Flow

The command pipeline is: 
MATLAB → ROS2 Node → Trajectory Topic → Controller → UR10 → Joint States → MATLAB

## Notes
• Uses fake hardware (no physical robot required)
• Designed for DevOps reproducibility
• Compatible with MATLAB ROS2 toolbox


