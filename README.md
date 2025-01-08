# openni2_camera

## Introduction
ROS wrapper for openni 2.0

Note: openni2_camera supports xtion devices, but not kinects. For using a kinect with ROS, try the freenect stack: http://www.ros.org/wiki/freenect_stack

## Contribution

Branching:
- ROS1:
   - Latest: [ros1](https://github.com/ros-drivers/openni2_camera/tree/ros1)
   - For ROS [Jade](http://wiki.ros.org/jade), [Indigo](http://wiki.ros.org/indigo) or earlier: [indigo-devel](https://github.com/ros-drivers/openni2_camera/tree/indigo-devel)
- ROS2:
   - The [ros2](https://github.com/ros-drivers/openni2_camera/tree/ros2) branch supports Jazzy and later
   - The [iron](https://github.com/ros-drivers/openni2_camera/tree/iron) branch supports Humble to Iron
   - openni2_launch has NOT been ported yet

## Developer document
   - [docs.ros.org/openni2_launch](http://docs.ros.org/en/melodic/api/openni2_launch/html/)
   - Source of the doc: [openni2_launch/doc](./openni2_launch/doc/)

## Running ROS2 Driver

An example launch exists that loads just the camera component:

```
ros2 launch openni2_camera camera_only.launch.py
```

If you want to get a PointCloud2, use:

```
ros2 launch openni2_camera camera_with_cloud.launch.py
```

## Migration from ROS1

 * The rgb/image topic has been renamed to rgb/image_raw for consistency.
 * The nodelet has been refactored into an rclcpp component called
   "openni2_wrapper::OpenNI2Driver". See the launch folder for an example
   of how to start this.
 * rgbd_launch and openni2_launch have not yet been ported (although it
   is now possible since lazy pub/sub is implemented). It is recommended
   to create a launch file with the specific pipeline you want.
   See the launch folder for an example.

## Known Issues

* Using "use_device_time" is currently broken.

* 01/08/2025: attempted fix for the following problem

```bash
--- stderr: openni2_camera
/home/icore-az/ros2_astra_ws/src/ros-drivers/openni2_camera/openni2_camera/src/openni2_driver.cpp: In member function ‘void openni2_wrapper::OpenNI2Driver::advertiseROSTopics()’:
/home/icore-az/ros2_astra_ws/src/ros-drivers/openni2_camera/openni2_camera/src/openni2_driver.cpp:162:33: error: ‘struct rclcpp::PublisherEventCallbacks’ has no member named ‘matched_callback’
  162 |     pub_options.event_callbacks.matched_callback =
      |                                 ^~~~~~~~~~~~~~~~
/home/icore-az/ros2_astra_ws/src/ros-drivers/openni2_camera/openni2_camera/src/openni2_driver.cpp:163:14: error: ‘rclcpp::MatchedInfo’ has not been declared
  163 |       [this](rclcpp::MatchedInfo&)
      |              ^~~~~~
/home/icore-az/ros2_astra_ws/src/ros-drivers/openni2_camera/openni2_camera/src/openni2_driver.cpp:169:58: error: too many arguments to function ‘image_transport::CameraPublisher image_transport::create_camera_publisher(rclcpp::Node*, const string&, rmw_qos_profile_t)’
  169 |     pub_color_ = image_transport::create_camera_publisher(this, "rgb/image_raw", custom_qos, pub_options);
      |                  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
In file included from /home/icore-az/ros2_astra_ws/src/ros-drivers/openni2_camera/openni2_camera/include/openni2_camera/openni2_driver.h:39,
                 from /home/icore-az/ros2_astra_ws/src/ros-drivers/openni2_camera/openni2_camera/src/openni2_driver.cpp:32:
/opt/ros/humble/include/image_transport/image_transport/image_transport.hpp:74:17: note: declared here
   74 | CameraPublisher create_camera_publisher(
      |                 ^~~~~~~~~~~~~~~~~~~~~~~
/home/icore-az/ros2_astra_ws/src/ros-drivers/openni2_camera/openni2_camera/src/openni2_driver.cpp:176:33: error: ‘struct rclcpp::PublisherEventCallbacks’ has no member named ‘matched_callback’
  176 |     pub_options.event_callbacks.matched_callback =
      |                                 ^~~~~~~~~~~~~~~~
/home/icore-az/ros2_astra_ws/src/ros-drivers/openni2_camera/openni2_camera/src/openni2_driver.cpp:177:14: error: ‘rclcpp::MatchedInfo’ has not been declared
  177 |       [this](rclcpp::MatchedInfo&)
      |              ^~~~~~
/home/icore-az/ros2_astra_ws/src/ros-drivers/openni2_camera/openni2_camera/src/openni2_driver.cpp:183:55: error: too many arguments to function ‘image_transport::CameraPublisher image_transport::create_camera_publisher(rclcpp::Node*, const string&, rmw_qos_profile_t)’
  183 |     pub_ir_ = image_transport::create_camera_publisher(this, "ir/image_raw", custom_qos, pub_options);
      |               ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
In file included from /home/icore-az/ros2_astra_ws/src/ros-drivers/openni2_camera/openni2_camera/include/openni2_camera/openni2_driver.h:39,
                 from /home/icore-az/ros2_astra_ws/src/ros-drivers/openni2_camera/openni2_camera/src/openni2_driver.cpp:32:
/opt/ros/humble/include/image_transport/image_transport/image_transport.hpp:74:17: note: declared here
   74 | CameraPublisher create_camera_publisher(
      |                 ^~~~~~~~~~~~~~~~~~~~~~~
/home/icore-az/ros2_astra_ws/src/ros-drivers/openni2_camera/openni2_camera/src/openni2_driver.cpp:190:33: error: ‘struct rclcpp::PublisherEventCallbacks’ has no member named ‘matched_callback’
  190 |     pub_options.event_callbacks.matched_callback =
      |                                 ^~~~~~~~~~~~~~~~
/home/icore-az/ros2_astra_ws/src/ros-drivers/openni2_camera/openni2_camera/src/openni2_driver.cpp:191:14: error: ‘rclcpp::MatchedInfo’ has not been declared
  191 |       [this](rclcpp::MatchedInfo&)
      |              ^~~~~~
/home/icore-az/ros2_astra_ws/src/ros-drivers/openni2_camera/openni2_camera/src/openni2_driver.cpp:197:62: error: too many arguments to function ‘image_transport::CameraPublisher image_transport::create_camera_publisher(rclcpp::Node*, const string&, rmw_qos_profile_t)’
  197 |     pub_depth_raw_ = image_transport::create_camera_publisher(this, "depth_raw/image", custom_qos, pub_options);
      |                      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
In file included from /home/icore-az/ros2_astra_ws/src/ros-drivers/openni2_camera/openni2_camera/include/openni2_camera/openni2_driver.h:39,
                 from /home/icore-az/ros2_astra_ws/src/ros-drivers/openni2_camera/openni2_camera/src/openni2_driver.cpp:32:
/opt/ros/humble/include/image_transport/image_transport/image_transport.hpp:74:17: note: declared here
   74 | CameraPublisher create_camera_publisher(
      |                 ^~~~~~~~~~~~~~~~~~~~~~~
/home/icore-az/ros2_astra_ws/src/ros-drivers/openni2_camera/openni2_camera/src/openni2_driver.cpp:198:58: error: too many arguments to function ‘image_transport::CameraPublisher image_transport::create_camera_publisher(rclcpp::Node*, const string&, rmw_qos_profile_t)’
  198 |     pub_depth_ = image_transport::create_camera_publisher(this, "depth/image", custom_qos, pub_options);
```
