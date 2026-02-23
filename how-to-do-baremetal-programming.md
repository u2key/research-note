# How to Do BareMetal Programming on DE10-Standard

## 1. Setup WSL2

```
sudo apt install make
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

## 4. Get Pre-Loader

```
wget https://download.terasic.com/downloads/cd-rom/de10-standard/Linux/DE10-Standard_Linux_Console.zip
```
```
unzip DE10-Standard_Linux_Console.zip
```
```

```

## 5. Open `Ashling RiscFree IDE for Altera 25.1std`

## 6. Set Workspace

```
C:\Users\admin\Documents\AshlingWorkspace\
```

## 7. Create a New Project

- File > New > C/C++ Project > C Project
  - Project name: `DE10_Standard_Beremetal`
  - Project type: `Empty Project`
    - Toolchains: `Arm Cross GCC`

## 8. Edit Settings

- File > Properties > C/C++ Build > Settings
  - Target Processor
    - Arm family (-mcpu): `cortex-a9`
    - Architecture (-march): `armv7-a`
    - Instruction set: `Arm (-marm)`
  - GNU Arm Cross Create Flush Image > General
    - Output file format (-O): `Raw binary`
  - GNU Arm C Compiler > Miscellaneous
    - Other compiler flags: `--specs=nosys.specs --static`

## 9. Create C Source File

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

## 10. Build Project to Generate DE10_Standard_Baremetal.bin

- Project > Build Project

## 11. Create Disk Image

```
sudo truncate -s 512M de10_sdcard.img
```
```
sudo losetup -P -f --show de10_sdcard.img
```
```
sudo parted -s /dev/loop0 mklabel msdos
```
```
sudo parted -s /dev/loop0 mkpart primary fat32 1MiB 101MiB
```
```
sudo parted -s /dev/loop0 mkpart primary 101MiB 102MiB
```
```
sudo sfdisk --part-type /dev/loop0 2 <<< "type=a2"
```
```
sudo losetup -d /dev/loop0
```
```
sudo losetup -P -f --show de10_sdcard.img
```
```
sudo mkfs.vfat /dev/loop0p1
```
```
mkdir -p mnt_fat
```
```
sudo mount /dev/loop0p1 mnt_fat
```
```
sudo cp DE10_Standard_Baremetal.bin mnt_fat
```
```
sudo umount mnt_fat
```
```
sudo dd if=preloader-mkimage.bin of=/dev/loop0p3 bs=64k status=progress
```
```
sudo sync
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
