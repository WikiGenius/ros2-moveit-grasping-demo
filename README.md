# ROS2 MoveIt Grasping Demo

## Overview
This repository contains a ROS 2 / MoveIt2 demo for perception-guided robotic grasping. The grasp target is localized using object detection, point-cloud processing, and computer vision, then the localized object pose is used to plan and execute a grasping sequence.

The execution layer plans arm motion, performs Cartesian approach and retreat motions, and controls a gripper through a repeatable task sequence. The goal is to make a compact public demo that shows how perception output can be connected to MoveIt2 manipulation stages instead of one-off scripts.

## Research/Engineering Motivation
Reliable pick-and-place behavior is a core building block for mobile manipulation, inspection, and autonomous robotic task execution. A robot must not only move to a predefined pose; it must detect or localize the object, transform that pose into the robot planning frame, and execute a safe manipulation sequence.

This project focuses on the engineering layer between perception and action: using point-cloud / computer-vision object localization to define the grasp target, then planning a safe pre-grasp pose, Cartesian approach, gripper closure, retreat, and return to a known state.

The demo is useful as a small testbed for experimenting with MoveIt2 planning parameters, Cartesian path execution, gripper actions, perception-to-pose interfaces, and reusable ROS 2 node structure.

## Features
- Perception-guided grasp workflow using object localization from point clouds and computer vision.
- ROS 2 node structure for arm and gripper control.
- MoveIt2 motion planning for pre-grasp and home poses.
- Cartesian approach and retreat stages.
- Gripper open/close action flow.
- Modular C++ targets for trajectory tests, gripper tests, approach/retreat behavior, and drawing motions.
- Demo media showing the manipulation sequence.

## Method
The task is decomposed into a perception-to-action sequence:

1. Acquire RGB/depth or point-cloud information from the scene.
2. Detect or segment the target object using computer vision / point-cloud processing.
3. Estimate the object pose and transform it into the robot planning frame.
4. Generate a pre-grasp pose relative to the localized object.
5. Move the arm to a home configuration.
6. Open the gripper.
7. Plan to the pre-grasp pose using MoveIt2.
8. Execute a Cartesian approach toward the object.
9. Close the gripper to grasp.
10. Execute a Cartesian retreat.
11. Release or return to a safe state.

The current MoveIt2 execution node consumes a pre-grasp pose and runs the approach/grasp/retreat sequence. In an integrated perception setup, that pose should come from the object detection and point-cloud localization pipeline.

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

The exact launch sequence depends on the robot description, MoveIt2 configuration, controllers, perception stack, camera/depth sensor, and simulation environment used with the package.

## Results
A demonstration GIF is included in the repository:

<img alt="ROS2 MoveIt grasping demo" src="./images/demo.gif" width="640">

## Limitations
- This is a simplified public demo, not a full production grasping pipeline.
- The public MoveIt2 node focuses on grasp execution after an object pose is available.
- Perception accuracy depends on camera calibration, point-cloud quality, object segmentation, frame transforms, and scene conditions.
- Robust grasp synthesis, perception uncertainty handling, and dynamic collision-scene updates should be expanded in future work.
- Robot-specific controllers and MoveIt2 configuration may need to be adapted for a different arm or gripper.
- Safety checks should be expanded before running on physical hardware.

## Roadmap
- [ ] Add a launch-first demo workflow.
- [ ] Add perception interface notes for object pose input.
- [ ] Add point-cloud / computer-vision localization example or diagram.
- [ ] Add baseline planning parameters and robot configuration notes.
- [ ] Add execution metrics such as planning time and Cartesian path fraction.
- [ ] Add experiment logs for successful and failed grasps.
- [ ] Add a short report or technical note explaining the perception-to-grasp pipeline.

## Citation / Acknowledgment
This project uses ROS 2 and MoveIt2. Please cite or acknowledge ROS 2, MoveIt2, and any robot-specific packages used when building on this work.

Knowledge and practical ROS/robotics course experience from [The Construct](https://www.theconstruct.ai/) helped support the learning behind this demo. The Construct provides ROS and robotics learning resources, courses, and hands-on training for robotics developers.

## Related Documentation
See [`docs/perception-to-grasp-pipeline.md`](docs/perception-to-grasp-pipeline.md) for the object-localization and grasp-execution pipeline summary.
