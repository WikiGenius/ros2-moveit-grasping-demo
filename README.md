# ROS2 MoveIt Grasping Demo

## Purpose

This repository contains a ROS2 / MoveIt2 demo for perception-guided robotic grasping. The grasp target is localized using object detection, point-cloud processing, and computer vision, then the localized object pose is used to plan and execute a grasping sequence.

The execution layer plans arm motion, performs Cartesian approach and retreat motions, and controls a gripper through a repeatable task sequence. The goal is to show how perception output can connect to MoveIt2 manipulation stages.

## Tested Environment

This repository is intended for ROS2 / MoveIt2 experimentation. The exact tested setup should be confirmed before claiming reproducibility.

- Ubuntu: to be confirmed
- ROS2 distribution: to be confirmed
- MoveIt2 version: to be confirmed
- Simulation or hardware: to be confirmed

## Current Runnable Artifact

The package includes C++ executables and launch files for MoveIt2 manipulation development:

- `approach_retreat`
- `test_gripper`
- `test_trajectory`
- `test_trajectory2`
- `test_trajectory3`
- `draw_x`

These commands require a compatible ROS2 / MoveIt2 workspace, robot model, controllers, planning scene, and any needed perception/object-pose input.

## Build and Run

Create or enter a ROS2 workspace, then clone this repository into `src`:

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
git clone https://github.com/WikiGenius/ros2-moveit-grasping-demo.git
cd ~/ros2_ws
rosdep install --from-paths src --ignore-src -r -y
colcon build --packages-select moveit2_scripts
source install/setup.bash
```

Run the approach/retreat executable:

```bash
ros2 run moveit2_scripts approach_retreat
```

Or use the related launch file:

```bash
ros2 launch moveit2_scripts approach_retreat.launch.py
```

Other development/test executables:

```bash
ros2 run moveit2_scripts test_gripper
ros2 run moveit2_scripts test_trajectory
ros2 run moveit2_scripts test_trajectory2
ros2 run moveit2_scripts test_trajectory3
ros2 run moveit2_scripts draw_x
```

Related launch files:

```bash
ros2 launch moveit2_scripts test_gripper.launch.py
ros2 launch moveit2_scripts test_trajectory.launch.py
ros2 launch moveit2_scripts test_trajectory2.launch.py
ros2 launch moveit2_scripts test_trajectory3.launch.py
ros2 launch moveit2_scripts draw_x.launch.py
```

The exact launch sequence depends on the robot description, MoveIt2 configuration, controllers, perception stack, camera/depth sensor, and simulation environment used with the package.

## What This Demonstrates

- Perception-guided grasp workflow using object localization from point clouds and computer vision.
- ROS2 node structure for arm and gripper control.
- MoveIt2 motion planning for pre-grasp and home poses.
- Cartesian approach and retreat stages.
- Gripper open/close action flow.
- Modular C++ targets for trajectory tests, gripper tests, approach/retreat behavior, and drawing motions.

A demonstration GIF is included in the repository:

<img alt="ROS2 MoveIt grasping demo" src="./images/demo.gif" width="640">

## Limitations

- This is a simplified public demo, not a full production grasping pipeline.
- The public MoveIt2 node focuses on grasp execution after an object pose is available.
- Perception accuracy depends on camera calibration, point-cloud quality, object segmentation, frame transforms, and scene conditions.
- Robust grasp synthesis, perception uncertainty handling, and dynamic collision-scene updates should be expanded in future work.
- Robot-specific controllers and MoveIt2 configuration may need to be adapted for a different arm or gripper.
- Safety checks should be expanded before running on physical hardware.
- No real-hardware validation is claimed in this README.

## Relation to My Research Direction

Reliable grasp execution is a building block for mobile manipulation, inspection, and active scanning. This repo supports the execution layer of the research portfolio by showing how localized perception outputs can be connected to robot motion planning, Cartesian motion stages, gripper control, and repeatable ROS2 / MoveIt2 task structure.

## Roadmap

- [ ] Add a launch-first demo workflow.
- [ ] Add perception interface notes for object pose input.
- [ ] Add point-cloud / computer-vision localization example or diagram.
- [ ] Add baseline planning parameters and robot configuration notes.
- [ ] Add execution metrics such as planning time and Cartesian path fraction.
- [ ] Add experiment logs for successful and failed grasps.
- [ ] Add a short report or technical note explaining the perception-to-grasp pipeline.

## Citation / Acknowledgment

This project uses ROS2 and MoveIt2. Please cite or acknowledge ROS2, MoveIt2, and any robot-specific packages used when building on this work.

Knowledge and practical ROS/robotics course experience from [The Construct](https://www.theconstruct.ai/) helped support the learning behind this demo. The Construct provides ROS and robotics learning resources, courses, and hands-on training for robotics developers.

## Related Documentation

See [`docs/perception-to-grasp-pipeline.md`](docs/perception-to-grasp-pipeline.md) for the object-localization and grasp-execution pipeline summary.
