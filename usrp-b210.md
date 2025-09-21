# USRP (Universal Software Radio Peripheral) B210

> ## References
> - https://kb.ettus.com/B200/B210/B200mini/B205mini/B206mini_Getting_Started_Guides
> - https://www.ettus.com/all-products/ub210-kit/
> - https://www.ettus.com/wp-content/uploads/2019/01/b200-b210_spec_sheet.pdf
> - http://haljion.net/index.php?option=com_content&view=article&id=519:wsl-ise-webpack&catid=120:2019-11-18-02-29-10

## 1. Specifications

- Dual Channel Transceiver (70 MHz - 6 GHz)
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

## 2. Install Xilinx ISE 14.7 VM Edition
### 2.1. Install Virtual Box 7.1.10
- https://download.virtualbox.org/virtualbox/7.1.10/VirtualBox-7.1.10-169112-Win.exe

### 2.2. Download Xilinx ISE Design Suite for Windows 10 and Windows 11 - 14.7 
- https://xilinx.com/downloadNav/vivado-design-tools/archive-ise.html

### 2.3. Disable Virtualization Check
```
notepad.exe C:\Users\admin\Downloads\Xilinx_ISE_14.7_Win10_14.7_VM_0213_1\bin\validate_virtualization_enabled.bat
```
```
@echo off
rem %SYSTEMROOT%\system32\windowspowershell\v1.0\powershell.exe -ExecutionPolicy Bypass -NoProfile -Command "& '%~dnp0.ps1'"
```

## 3. USRP Hardware Driver (UHD)
### 3.1. Install Required Packages
```
sudo apt install autoconf automake build-essential ccache cmake cpufrequtils doxygen ethtool g++ gcc git inetutils-tools libboost-all-dev libncurses5-dev libusb-1.0-0 libusb-1.0-0-dev libusb-dev python3-dev python3-mako python3-numpy python3-pip python3-requests python3-scipy python3-setuptools python3-ruamel.yaml
```

### 3.2. Build UHD from Source
```
cd ${HOME}
```
```
git clone https://github.com/EttusResearch/uhd.git
```
```
cd ${HOME}/uhd/
```
```
git checkout UHD-4.9
```
```
mkdir ${HOME}/uhd/host/build
```
```
cd ${HOME}/uhd/host/build
```
```
cmake -DCMAKE_INSTALL_PREFIX=/opt/uhd -DENABLE_C_API=ON -DENABLE_PYTHON_API=ON ../
```
```

```
```
make
```
```
[  0%] Building CXX object lib/deps/rpclib/CMakeFiles/uhd_rpclib.dir/lib/rpc/dispatcher.cc.o
[  0%] Building CXX object lib/deps/rpclib/CMakeFiles/uhd_rpclib.dir/lib/rpc/server.cc.o
[  0%] Building CXX object lib/deps/rpclib/CMakeFiles/uhd_rpclib.dir/lib/rpc/client.cc.o
...
[100%] Built target pyuhd
[100%] Generating build/timestamp
[100%] Built target usrp_mpm
[100%] Built target copy_mpm_packages
[100%] Generating build/timestamp
Including packages in pyuhd: ['usrp_mpm', 'uhd', 'usrp_mpm.xports', 'usrp_mpm.simulator', 'usrp_mpm.dboard_manager', 'usrp_mpm.sys_utils', 'usrp_mpm.periph_manager', 'uhd.usrp', 'uhd.usrp_clock', 'uhd.usrpctl', 'uhd.rfnoc_utils', 'uhd.dsp', 'uhd.utils', 'uhd.usrp.chips', 'uhd.usrp.cal', 'uhd.usrpctl.commands', 'uhd.rfnoc_utils.templates', 'uhd.rfnoc_utils.templates.modules', 'uhd.rfnoc_utils.modtool_commands']
[100%] Built target pyuhd_library
```
```
make test
```
```
Running tests...
Test project /home/ubuntu/uhd/host/build
      Start  1: actions_test
 1/98 Test  #1: actions_test .....................   Passed    0.15 sec
...
      Start 98: custom_reg_test
98/98 Test #98: custom_reg_test ..................   Passed    0.02 sec

100% tests passed, 0 tests failed out of 98

Total Test time (real) =  20.36 sec
```
```
sudo make install
```
```
[  1%] Built target uhd_rpclib
...
[100%] Built target pyuhd_library
Install the project...
-- Install configuration: "Release"
...
-- Installing: /opt/uhd/bin/usrp_hwd.py
```

### 3.3. Download the Images Package
```
sudo /opt/uhd/lib/uhd/utils/uhd_images_downloader.py
```
```
[INFO] Using base URL: https://files.ettus.com/binaries/cache/
[INFO] Images destination: /opt/uhd/share/uhd/images
[INFO] No inventory file found at /opt/uhd/share/uhd/images/inventory.json. Creating an empty one.
32108 kB / 32108 kB (100%) x4xx_x410_fpga_default-gccf87eb.zip
54042 kB / 54042 kB (100%) x4xx_x440_fpga_default-gccf87eb.zip
19776 kB / 19776 kB (100%) x3xx_x300_fpga_default-gccf87eb.zip
21659 kB / 21659 kB (100%) x3xx_x310_fpga_default-gccf87eb.zip
01129 kB / 01129 kB (100%) e3xx_e310_sg1_fpga_default-gccf87eb.zip
01116 kB / 01116 kB (100%) e3xx_e310_sg3_fpga_default-gccf87eb.zip
10216 kB / 10216 kB (100%) e3xx_e320_fpga_default-gccf87eb.zip
14288 kB / 14288 kB (100%) n3xx_n300_fpga_default-gccf87eb.zip
20918 kB / 20918 kB (100%) n3xx_n310_fpga_default-gccf87eb.zip
27298 kB / 27298 kB (100%) n3xx_n320_fpga_default-gccf87eb.zip
00479 kB / 00479 kB (100%) b2xx_b200_fpga_default-gc37b318.zip
00870 kB / 00870 kB (100%) b2xx_b210_fpga_default-gc37b318.zip
00485 kB / 00485 kB (100%) b2xx_b200mini_fpga_default-gc37b318.zip
00507 kB / 00507 kB (100%) b2xx_b205mini_fpga_default-gc37b318.zip
00507 kB / 00507 kB (100%) b2xx_b205mini_fpga_default-gc37b318.zip
00167 kB / 00167 kB (100%) b2xx_common_fw_default-g7f7d016.zip
00007 kB / 00007 kB (100%) usrp2_usrp2_fw_default-g6bea23d.zip
00450 kB / 00450 kB (100%) usrp2_usrp2_fpga_default-g6bea23d.zip
02415 kB / 02415 kB (100%) usrp2_n200_fpga_default-g6bea23d.zip
00009 kB / 00009 kB (100%) usrp2_n200_fw_default-g6bea23d.zip
02757 kB / 02757 kB (100%) usrp2_n210_fpga_default-g6bea23d.zip
00009 kB / 00009 kB (100%) usrp2_n210_fw_default-g6bea23d.zip
00319 kB / 00319 kB (100%) usrp1_usrp1_fpga_default-g6bea23d.zip
00522 kB / 00522 kB (100%) usrp1_b100_fpga_default-g6bea23d.zip
00006 kB / 00006 kB (100%) usrp1_b100_fw_default-g6bea23d.zip
00017 kB / 00017 kB (100%) octoclock_octoclock_fw_default-g14000041.zip
00025 kB / 00025 kB (100%) usb_common_windrv_default-gccf87eb.zip
[INFO] Images download complete.
```

### 3.4. Register Library Path
```
sudo vim /etc/ld.so.conf.d/uhd.conf
```
```
/opt/uhd/lib
```
```
sudo ldconfig
```

### 3.5. Add Executable Files Directory to PATH
```
vim ${HOME}/.bashrc
```
```
export PATH="${PATH}:/opt/uhd/bin/"
```

### Install `uhd` Python Package
```
cd ${HOME}/uhd/host/python/
```
```

```

### 3.6. Add UDEV Rule
```
sudo vim /etc/udev/rules.d/99-usrp.rules
```
```
SUBSYSTEM=="usb",ATTR{idVendor}=="2500",ATTR{idProduct}=="0020",MODE="0666"
```
```
sudo udevadm control --reload-rules
```
```
sudo udevadm trigger
```

## 4. Common Function of UHD
### 4.1. Find Connected USRP Devices
```
uhd_find_devices
```

### 4.2. Display Detailed Information of Connected USRP Devices 
```
uhd_usrp_probe
```
