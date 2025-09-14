# USRP (Universal Software Radio Peripheral) B210

> ## References
> [1] https://www.ettus.com/all-products/ub210-kit/
>
> [2] https://www.ettus.com/wp-content/uploads/2019/01/b200-b210_spec_sheet.pdf

## 1. Specifications
- Dual Channel Transceiver (70 MHz - 6GHz)
- AD9361 RFIC direct conversion transceiver
  - Providing up to 56MHz of real-time bandwidth
- Spartan6 XC6SLX150 FPGA
- USRP Hardware Driver software
  - Developing with GNU Radio
  - Developing GSM base station with OpenBTS
  - Developing seamless transition code
- RF frontend
  - Analog Devices AD9361
  - Real time throughput benchmarked at 61.44MS/s quadrature
  - Streaming up to 56 MHz of real-time RF bandwidth

## 2. Prepare Ubuntu 21.04 with Docker
```
docker run --name "USRP-B210" -it --net host -e DISPLAY=$DISPLAY -v /lib/modules:/lib/modules -v /usr/src:/usr/src -v /tmp/.X11-unix:/tmp/.X11-unix -v /home:/mnt/home ubuntu:21.04
```
```
cat /etc/os-release
```
```
NAME="Ubuntu"
VERSION="21.04 (Hirsute Hippo)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 21.04"
VERSION_ID="21.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=hirsute
UBUNTU_CODENAME=hirsute
```
```
sed -i -E 's/(jp.)?archive.ubuntu.com\/ubuntu/old-releases.ubuntu.com\/ubuntu/g' /etc/apt/sources.list
```
```
sed -i -E 's/(jp.)?security.ubuntu.com\/ubuntu/old-releases.ubuntu.com\/ubuntu/g' /etc/apt/sources.list
```
```
apt install bash-completion bison build-essential cpio dwarves flex gcc gdb libelf-dev libfontconfig1 libfreetype6 libglib2.0-0 libncurses5 libsm6 libssl-dev libuhd-dev libx11-6 libxi6 libxrandr2 libxrender1 linux-source make qemu-utils ssh sudo uhd-host usbutils vim wget
```

## 3. Install Xilinx ISE 14.7 
- Download ISE Sources to `~/xilinx`
  - https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vivado-design-tools/archive-ise.html

```
cd ~/xilinx/
```
```
tar -xvf Xilinx_ISE_DS_14.7_1015_1-1.tar
```
```
sudo ~/xilinx/xsetup
```
```
...
Execute: /opt/Xilinx/14.7/ISE_DS/PlanAhead/data/webtalk/webtalk_install.sh on
Execute: /bin/sh -c "/opt/Xilinx/14.7/ISE_DS/common/bin/lin64/install_script/install_drivers/install_drivers >>/opt/Xilinx/14.7/ISE_DS/.xinstall/install.log /opt/Xilinx/14.7/ISE_DS/ISE"
Driver installation failed. Please check the /.xinstall/install.log file for more information on the cause of the installation failure
Execute: Version Equalizer
Execute: /opt/Xilinx/14.7/ISE_DS/common/bin/lin64/xlcm
Xilinx Installer/Updater exited with return status "0".
################################################################
```
```
sudo vim /opt/Xilinx/14.7/ISE_DS/PlanAhead/data/webtalk/webtalk_install.sh
```
```bash
...
if [ $# != 1 ]; then
  echo "Specify on or off"
elif [ "$1" = "on" ]; then
...
elif [ "$1" = "off" ]; then
...
else
  echo "Valid options are on and off"
fi
```
```
sudo /opt/Xilinx/14.7/ISE_DS/PlanAhead/data/webtalk/webtalk_install.sh on
```
```
sudo vim /opt/Xilinx/14.7/ISE_DS/common/bin/lin64/install_script/install_drivers/linux_drivers/windriver64/windrvr/configure
```
```bash
...
if test $VER_BASE = "2.6" -a $VER_SUBMINOR -ge 33; then
  echo "#include <generated/autoconf.h>" >> hello.c
else if test $VER_BASE = "2.6" -a $VER_SUBMINOR -ge 17; then
  echo "#include <linux/autoconf.h>" >> hello.c
# else
#   echo "#include <linux/config.h>" >> hello.c
fi
...
```
```
sudo vim /opt/Xilinx/14.7/ISE_DS/common/bin/lin64/install_script/install_drivers/linux_drivers/windriver64/windrvr/configure.wd
```
```
...
INCLUDEDIRS="$INCLUDEDIRS -I$ROOT_DIR/include -I$ROOT_DIR"

CFLAGS="$CFLAGS -fno-pie"

EXTRA_CFLAGS="$EXTRA_CFLAGS $INCLUDEDIRS"
EXTRA_CFLAGS="$EXTRA_CFLAGS "
,,,
```
```
sudo ln -s /usr/src/linux-source-5.11.0 /usr/src/linux
```
```
sudo ln -s /lib/modules/$(uname -r)/build/include/generated/utsrelease.h /lib/modules/$(uname -r)/build/include/linux
```

## 3. MATLAB Setup
## 3.1. Install MATLAB 2012a

