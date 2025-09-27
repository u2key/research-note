# USRP (Universal Software Radio Peripheral) B210

> ## References
> - https://kb.ettus.com/B200/B210/B200mini/B205mini/B206mini_Getting_Started_Guides
> - https://www.ettus.com/all-products/ub210-kit/
> - https://www.ettus.com/wp-content/uploads/2019/01/b200-b210_spec_sheet.pdf
> - https://files.ettus.com/manual/md_fpga.html
> - http://haljion.net/index.php?option=com_content&view=article&id=519:wsl-ise-webpack&catid=120:2019-11-18-02-29-10
> - https://www.zep.co.jp/nbeppu/article/z-usrp-da1/

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
#### Ubuntu
```
sudo apt update && sudo apt upgrade && sudo apt install autoconf automake build-essential ccache cmake cpufrequtils doxygen ethtool g++ gcc gir1.2-gtk-3.0 git gobject-introspection inetutils-tools libboost-all-dev libncurses5-dev libuhd-dev libusb-1.0-0 libusb-1.0-0-dev libusb-dev python3-dev python3-gi python3-mako python3-numpy python3-pip python3-requests python3-scipy python3-setuptools python3-ruamel.yaml uhd-host
```

### 3.2. Download the Images Package
```
sudo /usr/libexec/uhd/utils/uhd_images_downloader.py
```
```
[INFO] Using base URL: https://files.ettus.com/binaries/cache/
[INFO] Images destination: /usr/share/uhd/images
[INFO] No inventory file found at /usr/share/uhd/images/inventory.json. Creating an empty one.
32419 kB / 32419 kB (100%) x4xx_x410_fpga_default-ge547a6b.zip
53458 kB / 53458 kB (100%) x4xx_x440_fpga_default-ge547a6b.zip
21602 kB / 21602 kB (100%) x3xx_x310_fpga_default-g6a990d9.zip
19738 kB / 19738 kB (100%) x3xx_x300_fpga_default-g6a990d9.zip
01126 kB / 01126 kB (100%) e3xx_e310_sg1_fpga_default-g6a990d9.zip
01118 kB / 01118 kB (100%) e3xx_e310_sg3_fpga_default-g6a990d9.zip
10196 kB / 10196 kB (100%) e3xx_e320_fpga_default-g6a990d9.zip
20842 kB / 20842 kB (100%) n3xx_n310_fpga_default-gd6608fc.zip
14257 kB / 14257 kB (100%) n3xx_n300_fpga_default-gd6608fc.zip
27244 kB / 27244 kB (100%) n3xx_n320_fpga_default-gd6608fc.zip
00481 kB / 00481 kB (100%) b2xx_b200_fpga_default-g92c09f7.zip
00464 kB / 00464 kB (100%) b2xx_b200mini_fpga_default-g92c09f7.zip
00883 kB / 00883 kB (100%) b2xx_b210_fpga_default-g92c09f7.zip
00511 kB / 00511 kB (100%) b2xx_b205mini_fpga_default-g92c09f7.zip
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
04839 kB / 04839 kB (100%) usb_common_windrv_default-g14000041.zip
[INFO] Images download complete.
```

### 3.2. Add UDEV Rule
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

### 4.3. Benchmarks
```
vim ${HOME}/.bashrc
```
```
export PATH="${PATH}:/usr/libexec/uhd/examples/"
```
```
benchmark_rate --rx_rate 10e6 --tx_rate 10e6
```

## 5. Generate FPGA Configuration Bitstream
### 5.1. Load License File
- Get ISE Embedded Edition License 
  - https://account.amd.com/en/forms/license/license-form.html

### 5.2. Get Sources from GitHub Repository
```
cd ${HOME}
```
```
git clone https://github.com/EttusResearch/uhd.git
```
```
git checkout UHD-4.9
```
```
cd ${HOME}/uhd/fpga/usrp3/top/b200/
```

### 5.3. Compile the Sources
```
sudo ln -s /usr/bin/python /usr/bin/python3
```
```
make B210
```

## 6. Configure FPGA
```
uhd_image_loader --args="type=b200,reset" --no-fw
```
```
Unit: USRP B210 (340E957)
[INFO] [UHD] linux; GNU C++ version 13.2.0; Boost_108300; UHD_4.6.0.0+ds1-5.1ubuntu0.24.04.1
[INFO] [B200] Loading FPGA image: /usr/share/uhd/images/usrp_b210_fpga.bin...
```
```
uhd_image_loader --args="type=b200" --fpga-path="/mnt/c/Users/admin/Documents/VirtualBoxShared/usrp_b210_fpga.bin"
```
```
Unit: USRP B210 (340E957)
[INFO] [UHD] linux; GNU C++ version 13.2.0; Boost_108300; UHD_4.6.0.0+ds1-5.1ubuntu0.24.04.1
[INFO] [B200] Loading FPGA image: /home/ubuntu/uhd/fpga/usrp3/top/b200/build/usrp_b210_fpga.bin...
```
