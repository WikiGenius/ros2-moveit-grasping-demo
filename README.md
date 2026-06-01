# ROS2 MoveIt Grasping Demo

## Overview
This repository contains a ROS 2 / MoveIt2 demo for executing a simple robotic arm grasping workflow. It plans arm motion, performs Cartesian approach and retreat motions, and controls a gripper through a repeatable task sequence.

The goal is to make a compact public demo that shows how manipulation behaviors can be organized into clear motion-planning stages instead of one-off scripts.

## Research/Engineering Motivation
Reliable pick-and-place behavior is a core building block for mobile manipulation, inspection, and autonomous robotic task execution. This project focuses on the engineering layer between high-level task intent and low-level robot commands: planning a safe approach, closing the gripper, retreating, and returning to a known state.

The demo is useful as a small testbed for experimenting with MoveIt2 planning parameters, Cartesian path execution, gripper actions, and reusable ROS 2 node structure.

## Features
- ROS 2 node structure for arm and gripper control.
- MoveIt2 motion planning for pre-grasp and home poses.
- Cartesian approach and retreat stages.
- Gripper open/close action flow.
- Modular C++ targets for trajectory tests, gripper tests, approach/retreat behavior, and drawing motions.
- Demo media showing the manipulation sequence.

## Method
The task is decomposed into a sequence of motion stages:

1. Move the arm to a home configuration.
2. Open the gripper.
3. Plan to a pre-grasp pose.
4. Execute a Cartesian approach toward the object.
5. Close the gripper to grasp.
6. Execute a Cartesian retreat.
7. Release or return to a safe state.

The implementation uses MoveIt2 planning interfaces and ROS 2 action/client patterns. Planning parameters such as velocity, acceleration, and timing can be adjusted for different robot setups.

## Installation
Create or enter a ROS 2 workspace, then clone this repository into `src`:

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
git clone https://github.com/WikiGenius/ros2-moveit-grasping-demo.git
cd ~/ros2_ws
rosdep install --from-paths src --ignore-src -r -y
colcon build --packages-select moveit2_scripts
source install/setup.bash
```

## Run
Build and source the workspace first, then run one of the available executables:

```bash
ros2 run moveit2_scripts approach_retreat
```

Other development/test executables in this package include:

```bash
ros2 run moveit2_scripts test_gripper
ros2 run moveit2_scripts test_trajectory
ros2 run moveit2_scripts test_trajectory2
ros2 run moveit2_scripts test_trajectory3
ros2 run moveit2_scripts draw_x
```

The exact launch sequence depends on the robot description, MoveIt2 configuration, controllers, and simulation environment used with the package.

## Results
A demonstration GIF is included in the repository:

<img alt="ROS2 MoveIt grasping demo" src="./images/demo.gif" width="640">

## Limitations
- This is a simplified public demo, not a full production grasping pipeline.
- Object detection, grasp synthesis, perception uncertainty, and collision-scene updates are not the main focus here.
- Robot-specific controllers and MoveIt2 configuration may need to be adapted for a different arm or gripper.
- Safety checks should be expanded before running on physical hardware.

## Roadmap
- [ ] Add a launch-first demo workflow.
- [ ] Add baseline planning parameters and robot configuration notes.
- [ ] Add execution metrics such as planning time and Cartesian path fraction.
- [ ] Add experiment logs for successful and failed grasps.
- [ ] Add a short report or technical note explaining the task pipeline.

## Citation / Acknowledgment
This project uses ROS 2 and MoveIt2. Please cite or acknowledge ROS 2, MoveIt2, and any robot-specific packages used when building on this work.
