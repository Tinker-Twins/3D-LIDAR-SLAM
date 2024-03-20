# ndt_slam

## Dependencies
```
sudo apt install ros-$ROS_DISTRO-tf2-sensor-msgs
sudo apt install ros-$ROS_DISTRO-jsk-rviz-plugins
```

## Setup
```
mkdir -p ~/catkin_ws/src && cd ~/catkin_ws/src
git clone https://github.com/RyuYamamoto/ndt_slam
cd .. && catkin_make
```

## Execute
```
roslaunch ndt_slam ndt_slam.launch
```
open other terminal
```
rosparam set use_sim_time true
rosbag play --clock <ROSBAG PATH>
```
open other terminal
```
rosservice call /ndt_slam_node/save_map 0.1 ~/map.pcd
```