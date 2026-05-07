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

##Run Simulation
ros2 launch gazebo_ros gazebo.launch.py gui:=false

##Run UR10 Control
ros2 launch ur_robot_driver ur_control.launch.py \
ur_type:=ur10 \
robot_ip:=192.168.0.1 \
use_fake_hardware:=true

##Control Interface
Publish trajectories to:
/scaled_joint_trajectory_controller/joint_trajectory
Subscribe to:
/joint_states

##Notes
• Uses fake hardware (no physical robot required)
• Designed for DevOps reproducibility
• Compatible with MATLAB ROS2 toolbox


