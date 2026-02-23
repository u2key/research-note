# How to Do BareMetal Programming on DE10-Standard

## 1. Setup WSL2

```
sudo apt install make u-boot-tools
```

## 2. Install Arm GNU Toolchain

- https://developer.arm.com/downloads/-/gnu-rm
  - arm-gnu-toolchain-15.2.rel1-mingw-w64-x86_64-arm-none-eabi.msi

## 3. Create Project Using Quartus

- Incluge HPS with Qsys
- Configure Parameters on SDRAM
  - Memory Parameters
    - Memory device speed grade: `800.0 MHz`
    - Total interface width: `32`
    - Row adress width: `15`
    - Column address width: `10`
    - Memory Initialization Options
      - Memory CAS latency setting: `11`
      - Output drive strength setting: `RZQ/7`
      - ODT Rtt normal value: `RZQ/4`
      - Memory write CAS latency setting: `8`

## 4. Open `Ashling RiscFree IDE for Altera 25.1std`

## 5. Set Workspace

```
C:\Users\admin\Documents\AshlingWorkspace\
```

## 6. Create a New Project

- File > New > C/C++ Project > C Project
  - Project name: `DE10_Standard_Beremetal`
  - Project type: `Empty Project`
    - Toolchains: `Arm Cross GCC`

## 7. Edit Settings

- File > Properties > C/C++ Build > Settings
  - Target Processor
    - Arm family (-mcpu): `cortex-a9`
    - Architecture (-march): `armv7-a`
    - Instruction set: `Arm (-marm)`
  - GNU Arm Cross Create Flush Image > General
    - Output file format (-O): `Raw binary`
  - GNU Arm C Compiler > Miscellaneous
    - Other compiler flags: `--specs=nosys.specs --static`

## 8. Create C Source File

- File > New > Source File
  - Source folder: `DE10_Standard_Beremetal`
  - Source file: `main.c`

```c
/*
 * main.c
 *
 *  Created on: 2026/02/23
 *      Author: u2key
 */

#include <stdint.h>

#define ADDRESS_HPS_UART_RX  0xFF708000 + 0x00000310
#define ADDRESS_HPS_UART_TX  0xFF708000 + 0x00000320

#define ADDRESS_FPGA_UART_RX 0xFF200000 + 0x00000000
#define ADDRESS_FPGA_UART_TX 0xFF200000 + 0x00000010

void _exit(int status) {
  while (1);
}

int main(void) {
  void *hps_uart_rx = (void *)(ADDRESS_HPS_UART_RX);
  void *hps_uart_tx = (void *)(ADDRESS_HPS_UART_TX);
  void *fpga_uart_rx = (void *)(ADDRESS_FPGA_UART_RX);
  void *fpga_uart_tx = (void *)(ADDRESS_FPGA_UART_TX);
  while (1) {
    *(char *)fpga_uart_rx = *(char *)hps_uart_rx;
    *(char *)hps_uart_tx = *(char *)fpga_uart_tx;
  }
  return 0;
}
```

## 9. Build Project to Generate DE10_Standard_Baremetal.bin

- Project > Build Project

## 10. Create Disk Image

```
wget https://download.terasic.com/downloads/cd-rom/de10-standard/Linux/DE10-Standard_LXDE.zip
```
```
unzip DE10-Standard_LXDE.zip
```
```
fdisk -l DE10_Standard_LXDE.img
```
```
Disk DE10_Standard_LXDE.img: 3.52 GiB, 3776970752 bytes, 7376896 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x55f3145b

Device                  Boot   Start     End Sectors  Size Id Type
DE10_Standard_LXDE.img1         4096 1028095 1024000  500M  b W95 FAT32
DE10_Standard_LXDE.img2      1028096 7376895 6348800    3G 83 Linux
DE10_Standard_LXDE.img3         2048    4095    2048    1M a2 unknown

Partition table entries are not in disk order.
```
```
mv DE10_Standard_LXDE.img de10_sdcard.img
```
```
sudo losetup -P -f --show de10_sdcard.img
```
```
mkdir -p mnt
```
```
sudo mount /dev/loop0p1 mnt
```
```
cp /mnt/c/Users/admin/Documents/AshlingWorkspace/DE10_Standard_Baremetal/Debug/DE10_Standard_Baremetal.bin mnt/
```
```
vim u-boot.txt
```
```
fatload mmc 0:1 $fpgadata soc_system.rbf;
fpga load 0 $fpgadata $filesize;
setenv fdtimage soc_system.dtb;
run bridge_enable_handoff;
fatload mmc 0:1 0x01000000 DE10_Standard_Baremetal.bin;
go 0x01000000;
#run mmcload;
#run mmcboot;
```
```
mkimage -A arm -O u-boot -T script -C none -a 0 -e 0 -n "Baremetal Boot Script" -d u-boot.txt u-boot.scr
```
```
Image Name:   Baremetal Boot Script
Created:      Mon Feb 23 17:07:52 2026
Image Type:   ARM U-Boot Script (uncompressed)
Data Size:    241 Bytes = 0.24 KiB = 0.00 MiB
Load Address: 00000000
Entry Point:  00000000
Contents:
   Image 0: 233 Bytes = 0.23 KiB = 0.00 MiB
```
```
cp u-boot.scr mnt/
```
```
sudo umount mnt
```
```
sudo losetup -d /dev/loop0
```
```
sudo dd if=de10_sdcard.img of=/dev/sde bs=4M status=progress
```
```
sudo sync
```
