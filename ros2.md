# ROS2
## 1. Build ROS2

> ### References
> - https://hellobreak.net/arduino-auto-robot-car/
> - https://hellobreak.net/raspberry-pi-pico-auto-robot-car/
> - https://qiita.com/ASAKA-219/items/ec17eaf4a7f4cc104dc6
> - https://note.com/kan_hatakeyama/n/n2aa17f80c369
> - https://note.com/npaka/n/nb8f38d4db221
> - https://docs.ros.org/en/humble/Installation/Alternatives/Ubuntu-Development-Setup.html
> - https://qiita.com/seshimaru/items/ed344530ead80ab1733f

### 1.1. Install ROS 2 Package
```
sudo apt update && sudo apt upgrade && sudo apt install software-properties-common
```
```
sudo add-apt-repository universe
```
```
sudo apt update && sudo apt upgrade && sudo apt install acl apt-utils bash-completion build-essential clang-format clang-tidy cppcheck curl doxygen git google-mock graphviz libacl1-dev libasio-dev libassimp-dev libbenchmark-dev libboost-system-dev libbullet-dev libconsole-bridge-dev libcurl4-openssl-dev libeigen3-dev libfreetype6-dev libgl1-mesa-dev libglew-dev libglu1-mesa-dev libgtest-dev libncurses-dev libopencv-dev liborocos-kdl-dev libpyside2-dev libqt5core5a libqt5gui5 libqt5opengl5 libqt5svg5 libqt5svg5-dev libqt5widgets5 libshiboken2-dev libspdlog-dev libsqlite3-dev libssl-dev libtinyxml-dev libtinyxml2-dev libx11-dev libxaw7-dev libxml2-utils libxrandr-dev libyaml-cpp-dev libyaml-dev libzstd-dev parted pciutils pkg-config pybind11-dev pydocstyle pyqt5-dev python3-argcomplete python3-babeltrace python3-cairo python3-flake8-blind-except python3-flake8-builtins python3-flake8-class-newline python3-flake8-comprehensions python3-flake8-deprecated python3-flake8-docstrings python3-flake8-import-order python3-flake8-quotes python3-lark python3-lttng python3-lxml python3-matplotlib python3-mypy python3-nose python3-numpy python3-pip python3-pip python3-psutil python3-pydot python3-pygraphviz python3-pykdl python3-pyqt5 python3-pyqt5.qtsvg python3-pyside2.qtsvg python3-pytest-cov python3-pytest-mock python3-pytest-repeat python3-pytest-rerunfailures python3-pytest-timeout python3-rospkg python3-sip-dev qt5-qmake qtbase5-dev shiboken2 software-properties-common ssh tango-icon-theme ufw uncrustify usbutils vim
```
```
curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list
```
```
sudo apt update && sudo apt upgrade && sudo apt install libignition-cmake2-dev libignition-math6-dev python3-colcon-common-extensions ros-dev-tools 
```

#### If you use arm64
> ```
> sudo apt update && sudo apt upgrade && sudo apt install ros-jazzy-desktop
> ```
> ```
> sudo rosdep init
> ```
> ```
> rosdep update
> ```
> ```
> echo ". /opt/ros/jazzy/setup.bash" >> ~/.bashrc
> ```

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

### 1.2. ROS 2 Demo
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
