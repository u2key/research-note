# USRP (Universal Software Radio Peripheral) B210

> ## References
> [1] https://www.ettus.com/all-products/ub210-kit/
>
> [2] https://www.ettus.com/wp-content/uploads/2019/01/b200-b210_spec_sheet.pdf
>
> [3] http://haljion.net/index.php?option=com_content&view=article&id=519:wsl-ise-webpack&catid=120:2019-11-18-02-29-10

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

## 2. Prepare Ubuntu 21.04 VM
### 2.1. Setup Virtual Machine
<div align="center">
  <table>
    <tr><td>Item</td><td>Value</td></tr>
    <tr><td>Virtual Box Version</td><td>7.1.10 r169112 (Qt6.5.3)</td></tr>
    <tr><td>Memory</td><td>8192 MB</td></tr>
    <tr><td>Storage</td><td>100.00 GB</td></tr>
  </table>
</div>

### 2.2. Guest OS Information
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

### 2.2. Update `/etc/apt/sources.list`
```
sudo sed -i -E 's/(jp.)?archive.ubuntu.com\/ubuntu/old-releases.ubuntu.com\/ubuntu/g' /etc/apt/sources.list
```
```
sudo sed -i -E 's/(jp.)?security.ubuntu.com\/ubuntu/old-releases.ubuntu.com\/ubuntu/g' /etc/apt/sources.list
```

### 2.3. Install Required Packages
```
sudo apt install apt-utils bash-completion bison build-essential cpio dwarves flex fxload gcc gdb git libc6-dev libelf-dev libfontconfig1 libfreetype6 libftdi-dev libglib2.0-0 libncurses5 libsm6 libssl-dev libuhd-dev libusb-dev libx11-6 libxi6 libxrandr2 libxrender1 make qemu-utils ssh sudo uhd-host usbutils vim wget
```

## 3. Install Xilinx ISE 14.7 

### 3.1. Download ISE Sources to `~/xilinx`
- https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vivado-design-tools/archive-ise.html

### 3.2. Extract Installer
```
cd ~/xilinx/
```
```
tar -xvf Xilinx_ISE_DS_14.7_1015_1-1.tar
```

### 3.3. Install Xilinx ISE 14.7 and Get Cable Driver Installation Failed Message
```
sudo ~/xilinx/xsetup
```
```
cat /opt/Xilinx/14.7/ISE_DS/.xinstall/install.log
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

### 3.4. Install Cable Driver
```
cd /opt/Xilinx
```
```
sudo git clone https://git.zerfleddert.de/git/usb-driver
```
```
cd /opt/Xilinx/usb-driver
```
```
sudo make
```
```
sudo ./setup_pcusb /opt/Xilinx/14.7/ISE_DS/ISE/
```

### 3.5. When Run ISE 14.7
```
export LD_PRELOAD=/opt/Xilinx/usb-driver/libusb-driver.so
```
```
. /opt/Xilinx/14.7/ISE_DS/settings64.sh
```

## 4. MATLAB Setup
## 4.1. Install MATLAB 2012a

