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
```
sudo apt update && sudo apt upgrade
```
```
sudo apt install curl
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
sudo apt install 
```

> ### References
> - https://hellobreak.net/arduino-auto-robot-car/
> - https://hellobreak.net/raspberry-pi-pico-auto-robot-car/
