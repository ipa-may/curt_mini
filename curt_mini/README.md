# This is the jazzy-devel branch

# Curt Mini

Select a branch and the ROS version for your robot. You may need a ROS1 and a ROS2 workspace.

## General Notes

This repository is used for setting up and starting the CURTmini software stack.
It consists of the configurations and dependencies for the sensor equipment on the robot.
In the bringup folder you find the launchfiles for starting the base and the whole navigation.


## Setting up the jazzy workspace

```
mkdir -p <colcon_ws>/src
cd <colcon_ws>/src 
git clone https://github.com/ipa320/curt_mini.git
cd ..
vcs import --recursive src/ < src/curt_mini/curt_mini/curt_mini.repos
vcs import --recursive src/ < src/curt_mini/ipa_ros2_control/ipa_ros2_control.repos
rosdep install --from-path src --ignore-src
colcon build
```





## Package layout

- `curt_mini_description` provides models, meshes, Xacro, and simulation configuration without the hardware driver dependency.
- `curt_mini_teleop` provides the joystick launch and configuration.
- `curt_mini` provides hardware bringup and depends on both packages and `ipa_ros2_control`.

Existing `ros2 launch curt_mini robot_base.launch.py` and joystick launch commands remain available. The joystick launch is a compatibility wrapper around `curt_mini_teleop`. Model consumers must replace `$(find curt_mini)/models/...` and `package://curt_mini/models/...` with the corresponding `curt_mini_description` paths. Simulation configuration now lives in `curt_mini_description/config`. Joystick configuration lives in `curt_mini_teleop/config`.

The driver launch accepts `controllers_file` for its controller manager configuration; existing direct invocations default to `curt_mini/config/ros2_control.yaml`.
