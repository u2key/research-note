# Autonomous Vehicle
## 1. Prepare Docker Container of ubuntu24.04:arm64 

> ### References
> - https://tah5.com/guides/ubuntu/docker/arm64/

### 1.1. Host Environment
```
cat /etc/os-release
```
```
PRETTY_NAME="Ubuntu 24.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04.3 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo
```

### 1.2. Install arm64 platform
```
docker run --privileged --rm tonistiigi/binfmt --install arm64
```

### 1.3. Create Docker Container
```
docker run --name "AutonomousVehicle" -it --platform linux/arm64 --net host -e DISPLAY=$DISPLAY -v /lib/modules:/lib/modules -v /usr/src:/usr/src -v /tmp/.X11-unix:/tmp/.X11-unix -v /home:/mnt/home ubuntu:24.04
```

## 2. Preparation for Development 

> ### References
> - https://hellobreak.net/arduino-auto-robot-car/
> - https://hellobreak.net/raspberry-pi-pico-auto-robot-car/
> - https://qiita.com/ASAKA-219/items/ec17eaf4a7f4cc104dc6
> - https://note.com/kan_hatakeyama/n/n2aa17f80c369
> - https://note.com/npaka/n/nb8f38d4db221
> - https://docs.ros.org/en/humble/Installation/Alternatives/Ubuntu-Development-Setup.html
> - https://qiita.com/seshimaru/items/ed344530ead80ab1733f

### 2.1. Install Required Packages
```
sudo apt update && sudo apt upgrade
```
```
sudo add-apt-repository universe
```
```
sudo apt install apt-utils bash-completion build-essential curl git libboost-system-dev parted pciutils python3-argcomplete python3-colcon-common-extensions python3-flake8-blind-except python3-flake8-builtins python3-flake8-class-newline python3-flake8-comprehensions python3-flake8-deprecated python3-flake8-docstrings python3-flake8-import-order python3-flake8-quotes python3-pytest-repeat python3-pytest-rerunfailures python3-pip python3-pytest-cov software-properties-common ssh usbutils vim
```

### 2.2. Resize rootfs partition
```
sudo parted
```
```
(parted) resizepart 2
End?  [3758MB]? 100%
```
```
sudo resize2fs /dev/mmcblk0p2
```
```
resize2fs 1.46.5 (30-Dec-2021)
Filesystem at /dev/mmcblk0p2 is mounted on /; on-line resizing required
old_desc_blocks = 1, new_desc_blocks = 8
The filesystem on /dev/mmcblk0p2 is now 15166464 (4k) blocks long.
```

### 2.3. Install ROS 2 Package
```
curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list
```
```
sudo apt update && sudo apt upgrade
```
```
sudo apt install ros-dev-tools ros-jazzy-desktop
```
```
sudo rosdep init
```
```
rosdep update
```
```
echo ". /opt/ros/jazzy/setup.bash" >> ~/.bashrc
```

#### (Optional) Build ROS 2 from Sources
```
sudo mkdir -p /opt/ros2_humble/src
```
```
sudo chmod 777 /opt/ros2_humble -R
```
```
cd /opt/ros2_humble
```
```
vcs import --input https://raw.githubusercontent.com/ros2/ros2/humble/ros2.repos src
```
```
=== src/ament/ament_cmake (git) ===
Cloning into '.'...
=== src/ament/ament_index (git) ===
Cloning into '.'...
...
=== src/ros2/yaml_cpp_vendor (git) ===
Cloning into '.'...
```
```
sudo rosdep init
```
```
Wrote /etc/ros/rosdep/sources.list.d/20-default.list
Recommended: please run
 rosdep update
```
```
rosdep update
```
```
reading in sources list data from /etc/ros/rosdep/sources.list.d
Hit https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/osx-homebrew.yaml
Hit https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/base.yaml
Hit https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/python.yaml
Hit https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/ruby.yaml
Hit https://raw.githubusercontent.com/ros/rosdistro/master/releases/fuerte.yaml
Query rosdistro index https://raw.githubusercontent.com/ros/rosdistro/master/index-v4.yaml
Skip end-of-life distro "ardent"
Skip end-of-life distro "bouncy"
Skip end-of-life distro "crystal"
Skip end-of-life distro "dashing"
Skip end-of-life distro "eloquent"
Skip end-of-life distro "foxy"
Skip end-of-life distro "galactic"
Skip end-of-life distro "groovy"
Add distro "humble"
Skip end-of-life distro "hydro"
Skip end-of-life distro "indigo"
Skip end-of-life distro "iron"
Skip end-of-life distro "jade"
Add distro "jazzy"
Skip end-of-life distro "kinetic"
Skip end-of-life distro "lunar"
Skip end-of-life distro "melodic"
Add distro "noetic"
Add distro "rolling"
updated cache in /home/ubuntu/.ros/rosdep/sources.cache
```
```
rosdep install --from-paths src --ignore-src -y --skip-keys "fastcdr rti-connext-dds-6.0.1 urdfdom_headers"
```
```
WARNING: ROS_PYTHON_VERSION is unset. Defaulting to 3
executing command [sudo -H apt-get install -y libcurl4-openssl-dev]
...
#All required rosdeps installed successfully
```
```
colcon build --symlink-install --parallel-workers 1 --packages-skip-build-finished
```
```
Starting >>> ament_package
Starting >>> ament_lint
...
Summary: 263 packages finished [6h 56min 50s]
  1 package failed: rviz_default_plugins
  156 packages had stderr output: ament_clang_format ament_clang_tidy ament_cmake_gen_version_h ament_copyright ament_cppcheck ament_cpplint ament_flake8 ament_index_cpp ament_index_python ament_lint ament_lint_cmake ament_mypy ament_package ament_pclint ament_pep257 ament_pycodestyle ament_pyflakes ament_uncrustify ament_xmllint camera_calibration_parsers camera_info_manager class_loader composition composition_interfaces demo_nodes_cpp demo_nodes_py domain_coordinator dummy_map_server dummy_sensors example_interfaces foonathan_memory_vendor geometry_msgs google_benchmark_vendor iceoryx_binding_c iceoryx_hoofs iceoryx_posh image_transport interactive_markers keyboard_handler laser_geometry launch launch_ros launch_testing launch_testing_ros launch_xml launch_yaml libstatistics_collector libyaml_vendor lifecycle map_msgs mcap_vendor message_filters mimick_vendor nav_msgs osrf_pycommon osrf_testing_tools_cpp performance_test_fixture pluginlib qt_gui_cpp rcl rcl_action rcl_interfaces rcl_lifecycle rcl_logging_interface rcl_logging_spdlog rcl_yaml_param_parser rclcpp rclcpp_action rclcpp_components rclcpp_lifecycle rclpy rcpputils rcutils resource_retriever rmw rmw_cyclonedds_cpp rmw_dds_common rmw_fastrtps_cpp rmw_fastrtps_dynamic_cpp rmw_fastrtps_shared_cpp rmw_implementation robot_state_publisher ros2action ros2bag ros2cli ros2component ros2doctor ros2interface ros2launch ros2lifecycle ros2multicast ros2node ros2param ros2pkg ros2run ros2service ros2test ros2topic rosbag2_compression rosbag2_compression_zstd rosbag2_cpp rosbag2_storage rosbag2_storage_default_plugins rosbag2_storage_mcap rosbag2_tests rosbag2_transport rosidl_cli rosidl_generator_cpp rosidl_runtime_c rosidl_runtime_cpp rosidl_runtime_py rosidl_typesupport_c rosidl_typesupport_cpp rosidl_typesupport_fastrtps_c rosidl_typesupport_fastrtps_cpp rosidl_typesupport_interface rpyutils rqt_console rqt_gui rqt_gui_py rqt_msg rqt_plot rttest rviz_common rviz_default_plugins rviz_ogre_vendor rviz_rendering rviz_rendering_tests sensor_msgs sensor_msgs_py shape_msgs sros2 statistics_msgs std_msgs test_msgs test_tracetools tf2 tf2_bullet tf2_eigen tf2_eigen_kdl tf2_geometry_msgs tf2_kdl tf2_msgs tf2_ros tf2_ros_py tf2_sensor_msgs tf2_tools tlsf_cpp tracetools tracetools_launch tracetools_read tracetools_test tracetools_trace trajectory_msgs urdfdom visualization_msgs
  82 packages not processed
```
```
colcon build --symlink-install --parallel-workers 1 --packages-skip-build-finished
```
```
Starting >>> rviz_default_plugins
Starting >>> rqt_bag
...
Summary: 12 packages finished [1min 53s]
  1 package failed: rviz_default_plugins
  7 packages aborted: action_tutorials_py camera_info_manager_py common_interfaces rosidl_typesupport_introspection_tests test_launch_testing test_osrf_testing_tools_cpp test_rmw_implementation
  6 packages had stderr output: iceoryx_introspection launch_pytest rqt_bag rviz_default_plugins test_launch_testing test_osrf_testing_tools_cpp
  63 packages not processed
```
```
colcon build --symlink-install --parallel-workers 1 --packages-skip-build-finished
```
```
Summary: 0 packages finished [9.05s]
```
```
echo ". /opt/ros/jazzy/setup.bash" >> ~/.bashrc
```

### 2.4. ROS 2 Demo
```
ros2 run demo_nodes_cpp talker 1> /dev/null 2> /dev/null &
```
```
ros2 run demo_nodes_cpp listener
```
```
[INFO] [1757211577.145358506] [listener]: I heard: [Hello World: 4]
[INFO] [1757211578.145332395] [listener]: I heard: [Hello World: 5]
[INFO] [1757211579.145333803] [listener]: I heard: [Hello World: 6]
[INFO] [1757211580.145241443] [listener]: I heard: [Hello World: 7]
^C[INFO] [1757211580.911429188] [rclcpp]: signal_handler(signum=2)
```
