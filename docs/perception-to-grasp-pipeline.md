# Perception-To-Grasp Pipeline

This note explains how object localization connects to the MoveIt2 grasping demo.

## Pipeline Summary

The grasping workflow is perception-guided:

1. Acquire RGB/depth or point-cloud data from the scene.
2. Detect or segment the target object using computer vision and point-cloud processing.
3. Estimate the object pose in the camera or sensor frame.
4. Transform the object pose into the robot planning frame, such as `base_footprint`.
5. Generate a pre-grasp pose relative to the object pose.
6. Use MoveIt2 to plan the arm motion to the pre-grasp pose.
7. Execute a Cartesian approach.
8. Close the gripper.
9. Retreat and return to a safe state.

## Current Public Demo Boundary

The public MoveIt2 package focuses on the execution side of the pipeline. The `approach_retreat` executable consumes a pre-grasp pose and runs the arm/gripper task sequence.

In a complete perception setup, the hard-coded example pose should be replaced by a pose estimated from object detection and point-cloud / computer-vision localization.

## Important Interfaces

Input to manipulation:

```text
geometry_msgs/msg/Pose pre_grasp_pose
```

Expected frame:

```text
base_footprint
```

The perception system should provide a stable object or pre-grasp pose in the same frame used by MoveIt2 planning.

## Why This Matters

For mobile manipulation, grasping is not only a motion-planning problem. The robot must first understand where the object is. Point clouds and computer vision provide the scene/object localization needed to turn perception into a manipulation target.

This repo demonstrates the action side of that perception-action loop:

```text
object detection + point cloud localization -> pre-grasp pose -> MoveIt2 plan -> approach -> grasp -> retreat
```

## Limitations

- The public code currently does not include a full object detector or point-cloud segmentation node.
- Object localization quality depends on calibration, sensor noise, segmentation quality, and transform accuracy.
- The collision scene should be updated from perception for more robust physical execution.
- Future work should add a small public-safe perception example or diagram.

## Acknowledgment

Learning resources and ROS/robotics courses from [The Construct](https://www.theconstruct.ai/) helped support the practical ROS and manipulation knowledge behind this demo.
