# ndt_localizer

## Setup
```bash
cd ROS1_Workspace && catkin_make
```

## Configuration

### `map_loader.launch`
Modify the `pcd_path` in `map_loader.launch` to point to the desired PCD map:
```xml
<arg name="pcd_path"  default="$(find ndt_localizer)/map/map.pcd"/>
```

### `voxel_grid_filter.launch`
Configure the LIDAR pointcloud topic in `voxel_grid_filter.launch`:
```xml
<arg name="points_topic" default="/velodyne_points" />
```

If your LIDAR data is sparse (e.g. VLP-16), set smaller `leaf_size` in `voxel_grid_filter.launch` like 1.0. If your LIDAR pointcloud is dense (e.g. VLP-32, Hesai Pander40P, HDL-64, etc.), set `leaf_size` between 2.0 and 3.0.

### `static_tf.launch`

Configure the `base_link_to_localizer` transform with the correct `frame id` for your LIDAR:
```xml
<node pkg="tf2_ros" type="static_transform_publisher" name="base_link_to_localizer" args="0 0 0 0 0 0 base_link velodyne"/>
```

### `ndt_localizer.launch`
Configure the main parameters for NDT algorithm in `ndt_localizer.launch`:
```xml
<arg name="trans_epsilon" default="0.05" />
<arg name="step_size" default="0.1" />
<arg name="resolution" default="2.0" />
<arg name="max_iterations" default="30.0" />
<arg name="converged_param_transform_probability" default="3.0" />
```
These default params work nice with 16, 32 and 64 channel LIDARs.

## Execution
> **Note:** Move the PCD map to the `map` drectory before execution

Open a terminal and launch `ndt_localizer`.
```bash
roslaunch ndt_localizer ndt_localizer.launch
```

Wait a few seconds for the PCD map to load. Then, give an initial pose of vehicle with `2D Pose Estimate` in `RViz`. This operation will send a reference initial pose to topic `/initialpose`.

Open another terminal and play a pre-recorded `rosbag` or start streming data from sensor.
```bash
rosparam set use_sim_time true
rosbag play --clock <ROSBAG_PATH>
```

The localizer updates estimated pose on `/ndt_pose` topic. The localizer also publishs a tf from `base_link` to `map`.