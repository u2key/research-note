# USRP (Universal Software Radio Peripheral) B210

> ## References
> [1] https://www.ettus.com/all-products/ub210-kit/
> [2] https://qiita.com/qawsed477/items/11e4248861fdf8c6a585

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

## 2. Compiler Setup
### 2.1. Prepare Ubuntu:21.04
```
docker run --name "Xilinx_ISE_14.7" -it --net host -e DISPLAY=$DISPLAY -v /lib/modules:/lib/modules -v /usr/src:/usr/src -v /tmp/.X11-unix:/tmp/.X11-unix -v $HOME:/home/$USER ubuntu:21.04
```
```
docker exec -it --privileged Xilinx_ISE_14.7 "/bin/bash"
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
sed -i -e 's/archive.ubuntu.com\/ubuntu/old-releases.ubuntu.com\/ubuntu/g' /etc/apt/sources.list
```
```
sed -i -e 's/security.ubuntu.com\/ubuntu/old-releases.ubuntu.com\/ubuntu/g' /etc/apt/sources.list
```
```
sudo apt install bash-completion bison build-essential cpio dwarves flex gcc gdb libelf-dev libfontconfig1 libfreetype6 libglib2.0-0 libncurses5 libsm6 libssl-dev libuhd-dev libx11-6 libxi6 libxrandr2 libxrender1 make qemu-utils sudo uhd-host usbutils vim wget
```
```
sudo mkdir -p /usr/lib/modules_overlay/work/$(uname -r)
```
```
sudo mkdir -p /usr/lib/modules_overlay/upper/$(uname -r)
```
```
sudo mount -t overlay overlay -o lowerdir=/usr/lib/modules/$(uname -r),upperdir=/usr/lib/modules_overlay/upper/$(uname -r),workdir=/usr/lib/modules_overlay/work/$(uname -r) /usr/lib/modules/$(uname -r)
```
```
sudo vim /etc/wsl.conf
```
```
[boot]
systemd=true
command=mount -t overlay overlay -o lowerdir=/usr/lib/modules/$(uname -r),upperdir=/usr/lib/modules_overlay/upper/$(uname -r),workdir=/usr/lib/modules_overlay/work/$(uname -r) /usr/lib/modules/$(uname -r); modprobe zfs, modprobe scsi_transport_iscsi; modprobe usb-storage; modprobe kheaders; modprobe act_gact; modprobe act_profile; modprobe sch_sfq;

[interop]
enabled=true
appendWindowsPath=true
```

### 2.1. Install ISE 14.7
- Download sources from: https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vivado-design-tools/archive-ise.html
  - Extract `Xilinx_ISE_DS_14.7_1015_1-1.tar` on `/opt/ise`
  - Save `Xilinx_ISE_DS_14.7_1015_1-{2,3,4}.zip.xz` to `/opt/Xilinx`

```
sudo /opt/ise/xsetup
```
