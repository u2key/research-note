# Build Petalinux for Zybo Z7

## 1. Download Petalinux v2025.1 Installer
- https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/embedded-design-tools.html

## 2. Prepare Ubuntu 24.04.3 LTS
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

## 3. Build Petalinux
### 3.1. Create directories: `/opt/petalinux` and `/opt/petalinux/build`.
```
sudo mkdir -p /opt/petalinux/build
```

### 3.2. Make `/opt/petalinux` and its below accessible from anyone
```
sudo chown ubuntu:ubuntu /opt/petalinux -R
```

### 3.3. Change current directory to `/opt/petalinux`
```
cd /opt/petalinux
```

### 3.4. Copy Petalinux Installer
```
cp /home/ubuntu/petalinux-v2025.1-05180714-installer.run /opt/petalinux/
```

### 3.5. Make Petalinux Installer executable
```
chmod +x petalinux-v2025.1-05180714-installer.run 
```

### 3.6. Add i386 architecture support
```
sudo dpkg --add-architecture i386
```

### 3.7. Change symbolic links: `/usr/bin/sh` and `/bin/sh`
```
sudo unlink /usr/bin/sh
```
```
sudo ln -s /usr/bin/bash /usr/bin/sh
```
```
sudo unlink /bin/sh
```
```
sudo ln -s /bin/bash /bin/sh
```
```
ls -l /usr/bin/sh /bin/sh
```
```
lrwxrwxrwx 1 root root 13 Jan 30 20:07 /bin/sh -> /usr/bin/bash
lrwxrwxrwx 1 root root 13 Jan 30 20:07 /usr/bin/sh -> /usr/bin/bash
```

### 3.8. Install required packages
```
sudo apt update
```
```
sudo apt install autoconf bc bind9-dnsutils bison build-essential chrpath flex gawk gcc-multilib git libglib2.0-dev libsdl1.2-dev libssl-dev libtinfo5 libncurses5-dev libtool linux-headers-generic linux-image-generic locales-all lsb-release pax rsync screen socat texinfo tftp-hpa tofrodos xterm xvfb xzip zlib1g-dev zlib1g-dev:i386
```

### 3.9. Re-Login to the user account
```
exit
```

### 3.10. Change current directory to `/opt/petalinux`
```
cd /opt/petalinux
```

### 3.11. Run Petalinux Installer
```
./petalinux-v2025.1-05180714-installer.run /opt/petalinux/build/
```
```
PetaLinux CMD tools installer version 2025.1
============================================
[WARNING] This is not a supported OS
[INFO] Checking free disk space
[INFO] Checking installed tools
[INFO] Checking installed development libraries
[INFO] Checking network and other services
[WARNING] No tftp server found - please refer to "UG1144  PetaLinux Tools Documentation Reference Guide" for its impact and solution

LICENSE AGREEMENTS

PetaLinux SDK contains software from a number of sources.  Please review
the following licenses and indicate your acceptance of each to continue.

You do not have to accept the licenses, however if you do not then you may
not use PetaLinux SDK.

Use PgUp/PgDn to navigate the license viewer, and press 'q' to close

Press Enter to display the license agreements
Do you accept Xilinx End User License Agreement? [y/N] > y
Do you accept Third Party End User License Agreement? [y/N] > y
Enter target directory for SDK (default: /opt/petalinux): /opt/petalinux/build/
[INFO] Installing PetaLinux SDK...done
[INFO] Setting it up...done
[INFO] Extracting xsct tarball...done
[INFO] PetaLinux SDK has been successfully set up and is ready to be used.
```

### 3.12. Import settings
```
source build/settings.sh
```
```
*************************************************************************************************************************************************
The PetaLinux source code and images provided/generated are for demonstration purposes only.
Please refer to https://xilinx-wiki.atlassian.net/wiki/spaces/A/pages/2741928025/Moving+from+PetaLinux+to+Production+Deployment
 for more details
*************************************************************************************************************************************************
PetaLinux environment set to '/opt/petalinux/build'
[WARNING] This is not a supported OS
[INFO] Checking free disk space
[INFO] Checking installed tools
[INFO] Checking installed development libraries
[INFO] Checking network and other services
[WARNING] No tftp server found - please refer to "UG1144 2025.1 PetaLinux Tools Documentation Reference Guide" for its impact and solution
```

### 3.13. Create Petalinux project
```
petalinux-create --type project --template zynq --name SampleProject
```
```
[NOTE] Argument: "--type project" has been deprecated. It is recommended to start using new python command line Argument.
[NOTE] Use: petalinux-create project [OPTIONS]
[INFO] Create project: SampleProject
[INFO] New project successfully created in /opt/petalinux/SampleProject
```

### 3.14. Change current directory to the created project directory
```
cd /opt/petalinux/SampleProject
```

### 3.15. Copy hardware design file
```
cp /home/ubuntu/system.xsa /opt/petalinux/
```

### 3.16. Configure the Petalinux project
```
petalinux-config --get-hw-description=../system.xsa
```
```
[INFO] Getting hardware description
[INFO] Extracting yocto SDK to components/yocto. This may take time!
[INFO] Bitbake is not available, some functionality may be reduced.
[INFO] Using HW file: /opt/petalinux/SampleProject/project-spec/hw-description/system.xsa
[INFO] Getting Platform info from HW file
[INFO] Generating Kconfig for project
[INFO] Menuconfig project
```

- `Subsystem Hardware Settings` > `Ethernet Settings`
  - Set `Ethernet MAC address` manually
- `FPGA Manager`
  - Select `FPGA Manager`
- `Image Packaging Configuration` > `Root filesystem type`
  - Select `EXT4 (SD/eMMC/SATA/USB)`

```
[INFO] Generating kconfig for rootfs
[INFO] Silentconfig rootfs
[INFO] Generating configuration files
[INFO] Adding user layers
[INFO] Generating machine conf file
[INFO] Generating plnxtool conf file
[INFO] Generating workspace directory
NOTE: Starting bitbake server...
NOTE: Started PRServer with DBfile: /opt/petalinux/SampleProject/build/cache/prserv.sqlite3, Address: 127.0.0.1:33951, PID: 28278
INFO: Specified workspace already set up, leaving as-is
INFO: Enabling workspace layer in bblayers.conf
[INFO] Successfully configured project
```

### 3.17. Configure Kernel
```
petalinux-config -c kernel
```
```
[INFO] Bitbake is not available, some functionality may be reduced.
[INFO] Using HW file: /opt/petalinux/SampleProject/project-spec/hw-description/system.xsa
[INFO] Getting Platform info from HW file
[INFO] Silentconfig project
[INFO] Silentconfig rootfs
[INFO] Generating configuration files
[INFO] Generating workspace directory
NOTE: Starting bitbake server...
NOTE: Started PRServer with DBfile: /opt/petalinux/SampleProject/build/cache/prserv.sqlite3, Address: 127.0.0.1:46661, PID: 28474
INFO: Specified workspace already set up, leaving as-is
[INFO] Configuring: kernel
[INFO] bitbake virtual/kernel -c cleansstate
NOTE: Started PRServer with DBfile: /opt/petalinux/SampleProject/build/cache/prserv.sqlite3, Address: 127.0.0.1:37167, PID: 28533
WARNING: XSCT has been deprecated. It will still be available for several releases. In the future, it's recommended to start new projects with SDT workflow.
WARNING: You are running bitbake under WSLv2, this works properly but you should optimize your VHDX file eventually to avoid running out of storage space
Loading cache: 100% |                                                       | ETA:  --:--:--
Loaded 0 entries from dependency cache.
Parsing recipes: 100% |######################################################| Time: 0:03:13
Parsing of 6138 .bb files complete (0 cached, 6138 parsed). 8821 targets, 1442 skipped, 28 masked, 0 errors.
NOTE: Resolving any missing task queue dependencies
NOTE: Fetching uninative binary shim file:///opt/petalinux/SampleProject/components/yocto/downloads/uninative/6bf00154c5a7bc48adbf63fd17684bb87eb07f4814fbb482a3fbd817c1ccf4c5/x86_64-nativesdk-libc-4.6.tar.xz;sha256sum=6bf00154c5a7bc48adbf63fd17684bb87eb07f4814fbb482a3fbd817c1ccf4c5 (will check PREMIRRORS first)
Sstate summary: Wanted 0 Local 0 Mirrors 0 Missed 0 Current 0 (0% match, 0% complete)
Initialising tasks: 100% |###################################################| Time: 0:00:00
NOTE: No setscene tasks
NOTE: Executing Tasks
NOTE: Tasks Summary: Attempted 3 tasks of which 0 didn't need to be rerun and all succeeded.

Summary: There were 2 WARNING messages.
[INFO] bitbake virtual/kernel -c menuconfig
NOTE: Started PRServer with DBfile: /opt/petalinux/SampleProject/build/cache/prserv.sqlite3, Address: 127.0.0.1:34175, PID: 29733
WARNING: XSCT has been deprecated. It will still be available for several releases. In the future, it's recommended to start new projects with SDT workflow.
Loading cache: 100% |########################################################| Time: 0:00:03
Loaded 8816 entries from dependency cache.
Parsing recipes: 100% |######################################################| Time: 0:00:00
Parsing of 6138 .bb files complete (6133 cached, 5 parsed). 8821 targets, 1442 skipped, 28 masked, 0 errors.
NOTE: Resolving any missing task queue dependencies
Checking sstate mirror object availability: 100% |###########################| Time: 0:00:04
Sstate summary: Wanted 230 Local 0 Mirrors 228 Missed 2 Current 0 (99% match, 0% complete):0
NOTE: Executing Tasks
```

- `Networking support` > `Wireless`
  - Set `cfg80211  - wireless configuration API` > `cfg80211 wireless extensions compatibility`: `Y`
  - Set `Generic IEEE 802.11 Networking Stack`: `Y`
- `Device Drivers`
  - Set `Parallel port support` > `PC-style hardware` > `Use FIFO/DMA if available`: `Y`
  - `Network device support` > `Wireless LAN` > `Realtek devices`
    - Set `Realtek 8180/8185/8187SE PCI support support`: `Y`
    - Set `Realtek 8187 and 8187B USB support`: `Y`
    - `Realtek rtlwifi family of devices`
      - Set `Realtek RTL8192CE/RTL8188CE Wireless Network Adapter`: `Y`
      - Set `Realtek RTL8192SE/RTL8191SE PCIe Wireless Network Adapter`: `Y`
      - Set `Realtek RTL8192DE/RTL8188DE PCIe Wireless Network Adapter`: `Y`
      - Set `Realtek RTL8723AE PCIe Wireless Network Adapter`: `Y`
      - Set `Realtek RTL8723BE PCIe Wireless Network Adapter`: `Y`
      - Set `Realtek RTL8188EE Wireless Network Adapter`: `Y`
      - Set `Realtek RTL8192EE Wireless Network Adapter`: `Y`
      - Set `Realtek RTL8821AE/RTL8812AE Wireless Network Adapter`: `Y`
      - Set `Realtek RTL8192CU/RTL8188CU USB Wireless Network Adapter`: `Y`
      - Set `Realtek RTL8192DU USB Wireless Network Adapter`: `Y`
  - Set `Realtek 802.11n USB wireless chips support`: `Y`
  - Set `Realtek 802.11ac wireless chips support` > `Realtek 8821CU USB wireless network adapter`: `Y`
- `Device Drivers` > `Character devices`
  - Set `Legacy (BSD) PTY support`: `Y`
  - Set `Non-standard serial port support`: `Y`
  - Set `Serial device bus`: `Y`
  - Set `Support for user-space parallel port device drivers`: `Y`
- `Device Drivers` > `Multimedia support` > `Media drivers` > `Media platform devices`
  - Set `Xilinx Video IP` > `Xilinx CSI-2 Rx Subsystem `: `Y`
  - Set `Xilinx Video HLS Core`: `Y`
- `Device Drivers` > `USB support`
  - Set `USB announce new devices`: `Y`
  - Set `USB 2.0 OTG FSM implementation`: `Y`
  - Set `xHCI HCD (USB 3.0) support`: `Y`
  - Set `OHCI HCD (USB 1.1) support` > `Generic OHCI driver for a platform device`: `Y`
  - Set `USB Modem (CDC, ACM) support`: `Y`
  - Set `USB Wireless Device Management support`: `Y`
  - Set `DesignWave USB3 DRD Core Support`: `Y`
  - Set `USB Serial Converter support` > `USB Serial Console device support`: `Y`
  - Set `USB Serial Converter support` > `USB Generic Serial Driver`: `Y`
  - Set `USB Serial Converter support` > `USB Serial Simple Driver`: `Y`
  - `USB Gadget Support`
    - Set `Debugging messages`: `Y`
    - Set `Debugging information files`: `Y`
    - Set `Serial gadget console support`: `Y`
    - Set `USB Gadget functions configurable through configfs` > `Abstract Control Model (CDC ACM)`: `Y`
    - Set `USB Gadget functions configurable through configfs` > `RNDIS`: `Y`
    - Set `USB Gadget functions configurable through configfs` > `Function filesystem (FunctionFS)`: `Y`
    - Set `USB Gadget functions configurable through configfs` > `HID function`: `Y`
    - Set `USB Gadget functions configurable through configfs` > `USB Webcam function`: `Y`
    - `USB Gadget precomposed configurations`
      - Set `Gadget Filesystem`: `Y`
      - Set `Function Filesystem` > `Include configuration with RNDIS (Ethernet)`: `Y`
      - Set `Mass Storage Gadget`: `Y`
      - Set `Serial Gadget (with CDC ACM and CDC OBEX support)`: `Y`
      - Set `CDC Composite Device (Ethernet and ACM)`: `Y`
      - Set `CDC Composite Device (ACM and mass storage)`: `Y`
      - Set `HID Gadget`: `Y`
      - Set `USB Webcam Gadget`: `Y`
      - Set `USB Raw Gadget`: `Y`
- Set `Device Drivers` > `PHY Subsystem` > `PHY Core`: `Y`
- Set `Device Drivers` > `On-Chip Interconnect management support`: `Y`

```
Changed configuration saved at:
 /opt/petalinux/SampleProject/build/tmp/work/zynq_generic_7z020-amd-linux-gnueabi/linux-xlnx/6.12.10+git/linux-zynq_generic_7z020-standard-build/.config
Recompile will be forced
NOTE: Tasks Summary: Attempted 770 tasks of which 757 didn't need to be rerun and all succeeded.

Summary: There was 1 WARNING message.
[INFO] bitbake virtual/kernel -c diffconfig
NOTE: Started PRServer with DBfile: /opt/petalinux/SampleProject/build/cache/prserv.sqlite3, Address: 127.0.0.1:41701, PID: 49460
WARNING: XSCT has been deprecated. It will still be available for several releases. In the future, it's recommended to start new projects with SDT workflow.
Loading cache: 100% |########################################################| Time: 0:00:05
Loaded 8816 entries from dependency cache.
Parsing recipes: 100% |######################################################| Time: 0:00:02
Parsing of 6138 .bb files complete (6133 cached, 5 parsed). 8821 targets, 1442 skipped, 28 masked, 0 errors.
NOTE: Resolving any missing task queue dependencies
Sstate summary: Wanted 68 Local 0 Mirrors 68 Missed 0 Current 74 (100% match, 100% complete)
Initialising tasks: 100% |###################################################| Time: 0:00:02
NOTE: Executing Tasks
Config fragment has been dumped into:
 /opt/petalinux/SampleProject/build/tmp/work/zynq_generic_7z020-amd-linux-gnueabi/linux-xlnx/6.12.10+git/fragment.cfg
NOTE: Tasks Summary: Attempted 476 tasks of which 475 didn't need to be rerun and all succeeded.

Summary: There was 1 WARNING message.
[INFO] recipetool appendsrcfile -wW /opt/petalinux/SampleProject/project-spec/meta-user virtual/kernel /opt/petalinux/SampleProject/build/tmp/work/zynq_generic_7z020-amd-linux-gnueabi/linux-xlnx/6.12.10+git/user_2025-09-19-09-11-00.cfg
NOTE: Starting bitbake server...
NOTE: Started PRServer with DBfile: /opt/petalinux/SampleProject/build/cache/prserv.sqlite3, Address: 127.0.0.1:43013, PID: 50551
WARNING: /opt/petalinux/SampleProject/components/yocto/layers/meta-qt5/lib/recipetool/create_qt5.py:133: SyntaxWarning: invalid escape sequence '\s'
  if re.match('^QT\s*[+=]+', line):

WARNING: /opt/petalinux/SampleProject/components/yocto/layers/meta-qt5/lib/recipetool/create_qt5.py:141: SyntaxWarning: invalid escape sequence '\s'
  elif re.match('^SUBDIRS\s*[+=]+', line):

WARNING: XSCT has been deprecated. It will still be available for several releases. In the future, it's recommended to start new projects with SDT workflow.
Loading cache: 100% |########################################################| Time: 0:00:07
Loaded 8816 entries from dependency cache.
Parsing recipes: 100% |######################################################| Time: 0:00:02
Parsing of 6138 .bb files complete (6133 cached, 5 parsed). 8821 targets, 1442 skipped, 28 masked, 0 errors.

Summary: There was 1 WARNING message.
NOTE: Writing append file /opt/petalinux/SampleProject/project-spec/meta-user/recipes-kernel/linux/linux-xlnx_%.bbappend
NOTE: Copying /opt/petalinux/SampleProject/build/tmp/work/zynq_generic_7z020-amd-linux-gnueabi/linux-xlnx/6.12.10+git/user_2025-09-19-09-11-00.cfg to /opt/petalinux/SampleProject/project-spec/meta-user/recipes-kernel/linux/linux-xlnx/user_2025-09-19-09-11-00.cfg
[INFO] bitbake virtual/kernel -c cleansstate
NOTE: Started PRServer with DBfile: /opt/petalinux/SampleProject/build/cache/prserv.sqlite3, Address: 127.0.0.1:36447, PID: 50633
WARNING: XSCT has been deprecated. It will still be available for several releases. In the future, it's recommended to start new projects with SDT workflow.
Loading cache: 100% |########################################################| Time: 0:00:07
Loaded 8816 entries from dependency cache.
Parsing recipes: 100% |######################################################| Time: 0:00:03
Parsing of 6138 .bb files complete (6126 cached, 12 parsed). 8821 targets, 1442 skipped, 28 masked, 0 errors.
NOTE: Resolving any missing task queue dependencies
Sstate summary: Wanted 0 Local 0 Mirrors 0 Missed 0 Current 0 (0% match, 0% complete)0:00:00
Initialising tasks: 100% |###################################################| Time: 0:00:00
NOTE: No setscene tasks
NOTE: Executing Tasks
NOTE: Tasks Summary: Attempted 3 tasks of which 0 didn't need to be rerun and all succeeded.

Summary: There was 1 WARNING message.
[INFO] Successfully configured kernel
```

### 3.18. Configure Device Tree
```
vim project-spec/meta-user/recipes-bsp/device-tree/files/system-user.dtsi
```
```
/include/ "system-conf.dtsi"

/{
  usb_phy0: usb_phy@0 {
    compatible = "ulpi-phy";
    #phy-cells = <0>;
    reg = <0xe0002000 0x1000>;
    view-port = <0x0170>;
    drv-vbus;
  };
};

&usb0 {
  compatible = "xlnx,zynq-usb-2.20a", "chipidea,usb2";
  status = "okay";
  clocks = <0x1 0x1c>;
  dr_mode = "host";
  interrupt-parent = <0x4>;
  interrupts = <0x0 0x15 0x4>;
  reg = <0xe0002000 0x1000>;
  usb-phy = <&usb_phy0>;
};
```

### 3.19. Build the Petalinux project
```
petalinux-build
```
```
[INFO] Building project
[INFO] Bitbake is not available, some functionality may be reduced.
[INFO] Using HW file: /opt/petalinux/SampleProject/project-spec/hw-description/system.xsa
[INFO] Getting Platform info from HW file
[INFO] Silentconfig project
[INFO] Silentconfig rootfs
[INFO] Generating configuration files
[INFO] Generating workspace directory
NOTE: Starting bitbake server...
NOTE: Started PRServer with DBfile: /opt/petalinux/SampleProject/build/cache/prserv.sqlite3, Address: 127.0.0.1:45885, PID: 51853
INFO: Specified workspace already set up, leaving as-is
[INFO] bitbake petalinux-image-minimal
NOTE: Started PRServer with DBfile: /opt/petalinux/SampleProject/build/cache/prserv.sqlite3, Address: 127.0.0.1:33751, PID: 51915
WARNING: XSCT has been deprecated. It will still be available for several releases. In the future, it's recommended to start new projects with SDT workflow.
Loading cache: 100% |########################################################| Time: 0:00:05
Loaded 8816 entries from dependency cache.
Parsing recipes: 100% |######################################################| Time: 0:00:02
Parsing of 6138 .bb files complete (6133 cached, 5 parsed). 8821 targets, 1442 skipped, 28 masked, 0 errors.
NOTE: Resolving any missing task queue dependencies
Checking sstate mirror object availability: 100% |###########################| Time: 0:00:35
Sstate summary: Wanted 2574 Local 0 Mirrors 2363 Missed 211 Current 125 (91% match, 92% complete)
NOTE: Executing Tasks
NOTE: Tasks Summary: Attempted 5950 tasks of which 5185 didn't need to be rerun and all succeeded.

Summary: There was 1 WARNING message.
[INFO] Failed to copy built images to tftp dir: /tftpboot
[INFO] Successfully built project
```

### 3.20. Generate boot image
```
petalinux-package --boot --force --fsbl images/linux/zynq_fsbl.elf --fpga images/linux/system.bit --u-boot
```
```
[INFO] File in BOOT BIN: "/opt/petalinux/SampleProject/images/linux/zynq_fsbl.elf"
[INFO] File in BOOT BIN: "/opt/petalinux/SampleProject/images/linux/system.bit"
[INFO] File in BOOT BIN: "/opt/petalinux/SampleProject/images/linux/u-boot.elf"
[INFO] File in BOOT BIN: "/opt/petalinux/SampleProject/images/linux/system.dtb"
[INFO] Generating zynq binary package BOOT.BIN...
[INFO]

****** Bootgen v2025.1-Merged
  **** Build date : Apr 26 2025-12:07:24
    ** Copyright 1986-2022 Xilinx, Inc. All Rights Reserved.
    ** Copyright 2022-2025 Advanced Micro Devices, Inc. All Rights Reserved.


[INFO]   : Bootimage generated successfully


[INFO] Binary is ready.
[INFO] Successfully Generated BIN File
[WARNING] Unable to access the TFTPBOOT folder /tftpboot!!!
[WARNING] Skip file copy to TFTPBOOT folder!!!
```

## 4. Create bootable microSD
### 4.1. Install Required Packages
```
sudo apt update && sudo apt upgrade && sudo apt install binfmt-support debootstrap dosfstools parted qemu-system qemu-user qemu-user-static usbutils
```

### 4.2. Change current directory to the created project directory
```
cd /opt/petalinux/SampleProject
```

### 4.3. Create 16.0 GB disk image
```
sudo truncate -s 16G image.img
```

### 4.4. Assign the disk image as loopback device
```
sudo losetup -f
```
```
/dev/loop0
```
```
sudo losetup /dev/loop0 image.img
```

### 4.5. Create a boot partition and a rootfs partition
```
sudo parted /dev/loop0
```
```
GNU Parted 3.6
Using /dev/loop0
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) unit MiB
(parted) mklabel msdos
(parted) mkpart primary 0MiB 100MiB
Warning: You requested a partition from 0.00MiB to 100MiB (sectors 0..204799).
The closest location we can manage is 0.00MiB to 100MiB (sectors 1..204799).
Is this still acceptable to you?
Yes/No? Yes
Warning: The resulting partition is not properly aligned for best performance: 1s % 2048s != 0s
Ignore/Cancel? Ignore
(parted) mkpart primary 100MiB 100%
(parted) p
Model: Loopback device (loopback)
Disk /dev/loop0: 16384MiB
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Disk Flags:

Number  Start    End       Size      Type     File system  Flags
 1      0.00MiB  100MiB    100MiB    primary
 2      100MiB   16384MiB  16284MiB  primary
```

### 4.6. Unassign the disk image
```
sudo losetup -d /dev/loop0
```

### 4.7. Re-assign the disk image
```
sudo losetup -P -f --show image.img
```
```
/dev/loop0
```
```
ls /dev/loop0*
```
```
/dev/loop0  /dev/loop0p1  /dev/loop0p2
```

### 4.8. Format the filesystems
```
sudo mkfs.vfat -F 32 /dev/loop0p1
```
```
mkfs.fat 4.2 (2021-01-31)
```
```
sudo mkfs.ext4 /dev/loop0p2
```
```
mke2fs 1.47.0 (5-Feb-2023)
Discarding device blocks: done
Creating filesystem with 4168704 4k blocks and 1042432 inodes
Filesystem UUID: db53e130-4b6e-41c7-9dea-93e138cb7be6
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
        4096000

Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done
```

### 4.9. Mount the filesystems
```
sudo mkdir -p /mnt/loop0p1 /mnt/loop0p2
```
```
sudo mount -t vfat /dev/loop0p1 /mnt/loop0p1
```
```
sudo mount -t ext4 /dev/loop0p2 /mnt/loop0p2
```

### 4.10. Copy boot image
```
cd /opt/petalinux/SampleProject/images/linux
```
```
sudo cp BOOT.BIN boot.scr image.ub system.bit /mnt/loop0p1
```

### 4.11. Create Ubuntu Jammy rootfs
```
sudo debootstrap --arch=armhf --foreign jammy /mnt/loop0p2
```
```
I: Retrieving InRelease
I: Checking Release signature
I: Valid Release signature (key id F6ECB3762474EDA9D21B7022871920D1991BC93C)
I: Retrieving Packages
I: Validating Packages
I: Resolving dependencies of required packages...
I: Resolving dependencies of base packages...
I: Checking component main on http://ports.ubuntu.com/ubuntu-ports...
I: Retrieving adduser 3.118ubuntu5
I: Validating adduser 3.118ubuntu5
I: Retrieving apt 2.4.5
I: Validating apt 2.4.5
...
I: Retrieving zlib1g 1:1.2.11.dfsg-2ubuntu9
I: Validating zlib1g 1:1.2.11.dfsg-2ubuntu9
I: Chosen extractor for .deb packages: dpkg-deb
I: Extracting base-files...
I: Extracting base-passwd...
...
I: Extracting util-linux...
I: Extracting zlib1g...
```

### 4.12. Setup the Ubuntu Jammy base system
```
sudo cp /usr/bin/qemu-arm-static /mnt/loop0p2/usr/bin
```
```
sudo update-binfmts --import qemu-arm
```
```
update-binfmts: warning: unable to open /usr/share/binfmts/qemu-arm: No such file or directory
update-binfmts: warning: couldn't find information about 'qemu-arm' to import
update-binfmts: exiting due to previous errors
```
```
sudo chroot /mnt/loop0p2
```
```
export LANG=C
```
```
/debootstrap/debootstrap --second-stage
```
```
I: Installing core packages...
I: Unpacking required packages...
I: Unpacking base-files...
...
I: Base system installed successfully.
```
```
mount none -t devpts /dev/pts
```
```
mount proc -t proc /proc
```

### 4.13. Setup user and exit
```
su
```
```
passwd
```
```
New password:
Retype new password:
passwd: password updated successfully
```
```
apt update && apt upgrade && apt install bash-completion bind9-dnsutils build-essential chrony dkms ethtool fdisk g++ gcc gdb gnupg lshw net-tools network-manager parted sudo usbutils vim wireless-tools
```
```
unlink /etc/resolv.conf
```
```
vim /etc/resolv.conf
```
```
nameserver 172.23.72.77
```
```
useradd -G sudo -s /bin/bash -p $(openssl passwd -6 ubuntu) ubuntu
```
```
vim /root/sudo.c
```
```
#include <stdio.h>

int main(int argc, char *argv[]) {
  printf("[sudo] ");
  for (int a = 0; a < argc; a++) {
    printf("%s ", argv[a]);
  }
  printf("\n");
  return 0;
}
```
```
gcc /root/sudo.c -o /usr/local/bin/sudo
```
```
mkhomedir_helper ubuntu
```
```
chown ubuntu:ubuntu /home/ubuntu
```
```
exit
```

### 4.14. Unmount the filesystems
```
sudo umount /mnt/loop0p1
```
```
sudo umount /mnt/loop0p2 
```

### 4.15. Unassign the disk image
```
sudo losetup -d /dev/loop0
```

### 4.16. Write the Disk Image
```
cd /opt/petalinux/SampleProject/
```
```
sudo dd if=image.img of=/dev/sde status=progress
```

### 4.17. (Optional) Chroot to SD Card
```
sudo mount /dev/sde2 /mnt/loop0p2
```
```
sudo chroot /mnt/loop0p2
```
```
mount none -t devpts /dev/pts
```
```
mount proc -t proc /proc
```
```
vim /root/sudo.c
```
```
#include <stdio.h>

int main(int argc, char *argv[]) {
  printf("[sudo] ");
  for (int a = 0; a < argc; a++) {
    printf("%s ", argv[a]);
  }
  printf("\n");
  return 0;
}
```
```
gcc /root/sudo.c -o /usr/local/bin/sudo
```

## 5. Console Access to Petalinux System
```
sudo usermod -aG dialout ubuntu
```
```
minicom -D /dev/ttyUSB1
```

## 6. Resize rootfs partition
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

## 7. Filesystem Mounting Settings
```
sudo vim /etc/init.d/remount-root
```
```
#! /bin/sh

mount -o rw,remount /
mount /dev/mmblk0p1 /boot
```

## 8. Setup Time
```
sudo apt install ntpdate
```
```
sudo timedatectl set-timezone Asia/Tokyo
```
```
sudo timedatectl set-ntp true
```
```
sudo ntpdate pool.ntp.org
```

## 9. Network Settings
### 9.1. Ethernet Setings
```
sudo vim /etc/init.d/assign-ip-address
```
```
#! /bin/sh

ip addr add 172.23.72.104/24 dev eth0
ip link set eth0 up
ip route add default via 172.23.72.1
ip link set eth0 up
```
```
sudo vim /etc/sysctl.conf
```
```
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
```
```
sudo sysctl -p
```

### 9.2. Wireless Settings
```
wget http://ftp.jp.debian.org/debian/pool/non-free-firmware/f/firmware-nonfree/firmware-realtek_20250917-1_all.deb
```
```
sudo dpkg -i firmware-realtek_20250917-1_all.deb
```
```
sudo systemctl restart NetworkManager
```
```
sudo nmcli device wifi connect "SSID" password "PASSWORD"
```

## References

- https://qiita.com/iwatake2222/items/ece0cdf83e6e1908fad8
- https://qiita.com/iwatake2222/items/6e6915f7318689818368
- https://zenn.dev/gnico/articles/2aef82b7adef44
- https://xilinx-wiki.atlassian.net/wiki/spaces/A/pages/2046001302/Building+Linux+usb+device+drivers+with+2021.1
- https://xilinx-wiki.atlassian.net/wiki/spaces/A/pages/18841645/Solution+Zynq+PL+Programming+With+FPGA+Manager?view=blog
- https://xilinx-wiki.atlassian.net/wiki/spaces/A/pages/18842272/Zynq+Linux+USB+Device+Driver
