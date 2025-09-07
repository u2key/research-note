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

```
sudo apt update && sudo apt upgrade
```
```
sudo apt install apt-utils bash-completion curl parted vim
```
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
echo ". /opt/ros/jazzy/setup.bash" >> ~/.bashrc
```
```
sudo rosdep init
```
```
rosdep update
```
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
