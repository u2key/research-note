# USRP (Universal Software Radio Peripheral) B210

> ## References
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

## 3. Install USRP Hardware Driver (UHD)
```
sudo apt install autoconf automake build-essential ccache cmake cpufrequtils doxygen ethtool g++ gcc git inetutils-tools libboost-all-dev libncurses5-dev libusb-1.0-0 libusb-1.0-0-dev libusb-dev python3-dev python3-mako python3-numpy python3-requests python3-scipy python3-setuptools python3-ruamel.yaml
```
```
cd
```
```
git clone https://github.com/EttusResearch/uhd.git
```
```
mkdir /home/ubuntu/uhd/host/build
```
```
cd /home/ubuntu/uhd/host/build
```
```
cmake -DCMAKE_INSTALL_PREFIX=/opt/uhd ../
```
```
make
```
```
make test
```
```
sudo make install
```
```
sudo ldconfig
```
