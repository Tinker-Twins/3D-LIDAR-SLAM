# 3D LIDAR SLAM | ROS

ROS Packages for Real-Time 3D LIDAR Based Simultaneous Localization and Mapping (SLAM) using Normal Distribution Transform (NDT) Scan Matching Algorithm.

## Dependencies
```bash
sudo apt install ros-$ROS_DISTRO-tf2-sensor-msgs
sudo apt install ros-$ROS_DISTRO-jsk-rviz-plugins
```

## Setup
```bash
cd ROS1_Workspace && catkin_make
```

## Execution
Open a terminal and launch `ndt_slam`.
```bash
roslaunch ndt_slam ndt_slam.launch
```
Open another terminal and play a pre-recorded `rosbag` or start streming data from sensor.
```bash
rosparam set use_sim_time true
rosbag play --clock <ROSBAG_PATH>
```
Open another terminal and call a service to save a PCD map of desired resolution to desired path.
```bash
rosservice call /ndt_slam_node/save_map 0.1 ~/map.pcd
```
