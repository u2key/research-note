# How to Do BareMetal Programming on DE10-Standard

## 1. Setup WSL2

```
sudo apt install make u-boot-tools
```

## 2. Install Required Software

- https://developer.arm.com/downloads/-/gnu-rm
  - arm-gnu-toolchain-15.2.rel1-mingw-w64-x86_64-arm-none-eabi.msi
 
- https://www.altera.com/download-center/license-agreement/78071/c9e1235c7f979a37d79eb57eee37596b030d209e?filename=SoCEDSSetup-20.1.0.711-linux.run
```
chmod 700 SoCEDSSetup-20.1.0.711-linux.run
```
```
./SoCEDSSetup-20.1.0.711-linux.run`
```

## 3. Co-Design Using Quartus

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
- Configure LOANIO
  - `h2f_loan_io` and `hps_io` created
  - `h2f_loan_io` is connected FPGA internal logic
  - `hps_io` is connected external pin 
- Generate RBF file
  - File > Convert Programming Files...
    - Programming file type: `Raw Binary File (.rbf)`
    - Configuration device: `EPCE16`
    - Mode: `Passive Parallel x16`
    - File name: `soc_system.rbf`
    - Input files to convert > SOF Data: `CoDesign.sof`

```verilog
module CoDesign
  (
    input         CLOCK_50,
    input  [ 9:0] SW,
    input  [ 3:0] KEY,
    output [ 9:0] LEDR,
    output [14:0] HPS_DDR3_A,
    output [ 2:0] HPS_DDR3_BA,
    output        HPS_DDR3_CAS_n,
    output        HPS_DDR3_CKE,
    output        HPS_DDR3_CK_n,
    output        HPS_DDR3_CK_p,
    output        HPS_DDR3_CS_n,
    output [ 3:0] HPS_DDR3_DM,
    inout  [31:0] HPS_DDR3_DQ,
    inout  [ 3:0] HPS_DDR3_DQS_N,
    inout  [ 3:0] HPS_DDR3_DQS_P,
    output        HPS_DDR3_ODT,
    output        HPS_DDR3_RAS_n,
    output        HPS_DDR3_RESET_n,
    output        HPS_DDR3_WE_n,
    input         HPS_DDR3_RZQ,
    inout         HPS_UART_RX,
    inout         HPS_UART_TX,
    inout         HPS_LED,
    inout         HPS_KEY
  );

  wire [66:0] h2f;
  wire [66:0] f2h;
  assign f2h[66:55] = 12'h000;
  assign f2h[   54] = 1'h0;   // HPS_KEY
  assign f2h[   53] = KEY[0]; // HPS_LED
  assign f2h[   52] = 1'h0;   //
  assign f2h[   51] = 1'h0;   //
  assign f2h[   50] = KEY[0]; // HPS_UART_TX
  assign f2h[   49] = 1'h0;   // HPS_UART_RX
  assign f2h[48: 0] = 49'h0000000000000;

  wire [66:0] f2h_en;
  assign f2h_en[66:55] = 12'h000;
  assign f2h_en[   54] = 1'h0; // HPS_KEY
  assign f2h_en[   53] = 1'h1; // HPS_LED
  assign f2h_en[   52] = 1'h0; //
  assign f2h_en[   51] = 1'h0; //
  assign f2h_en[   50] = 1'h1; // HPS_UART_TX
  assign f2h_en[   49] = 1'h0; // HPS_UART_RX
  assign f2h_en[48: 0] = 49'h0000000000000;

  assign LEDR[0] = h2f[49];
  assign LEDR[1] = KEY[0];
  assign LEDR[9] = h2f[54];

  hps hps0 (
    .clk_clk(CLOCK_50),
    .reset_reset_n(1'b1),
    .hps_io_hps_io_gpio_inst_LOANIO49(HPS_UART_RX),
    .hps_io_hps_io_gpio_inst_LOANIO50(HPS_UART_TX),
    .hps_io_hps_io_gpio_inst_LOANIO53(HPS_LED),
    .hps_io_hps_io_gpio_inst_LOANIO54(HPS_KEY),
    .loan_io_in(h2f),
    .loan_io_out(f2h),
    .loan_io_oe(f2h_en),
    .memory_mem_a(HPS_DDR3_A),
    .memory_mem_ba(HPS_DDR3_BA),
    .memory_mem_ck(HPS_DDR3_CK_p),
    .memory_mem_ck_n(HPS_DDR3_CK_n),
    .memory_mem_cke(HPS_DDR3_CKE),
    .memory_mem_cs_n(HPS_DDR3_CS_n),
    .memory_mem_ras_n(HPS_DDR3_RAS_n),
    .memory_mem_cas_n(HPS_DDR3_CAS_n),
    .memory_mem_we_n(HPS_DDR3_WE_n),
    .memory_mem_reset_n(HPS_DDR3_RESET_n),
    .memory_mem_dq(HPS_DDR3_DQ),
    .memory_mem_dqs(HPS_DDR3_DQS_P),
    .memory_mem_dqs_n(HPS_DDR3_DQS_N),
    .memory_mem_odt(HPS_DDR3_ODT),
    .memory_mem_dm(HPS_DDR3_DM),
    .memory_oct_rzqin(HPS_DDR3_RZQ),
    //.uart_rx_export(uart_rx),
    //.uart_tx_export(uart_tx)
  );

endmodule
```

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

## 7. Source Files

- File > New > Source File
  - Source folder: `DE10_Standard_Beremetal`
  - Source file: `main.c`

```c
#include <stdio.h>
#include <stdint.h>

#define SYSMGR_FRZCTRL_VIOCTRL    0xFFD08040
#define SYSMGR_PINMUX_HPS_UART_RX 0xFFD084C4
#define SYSMGR_PINMUX_HPS_UART_TX 0xFFD084C8
#define SYSMGR_PINMUX_HPS_LED     0xFFD084D4
#define SYSMGR_PINMUX_HPS_KEY     0xFFD084D8
#define SYSMGR_SCANMGR_LOANIO1    0xFFD080E4 // LOANIO1: pin 29 - pin 57

void _exit(int status) {
  while (1);
}
void _close(void) {
}
void _lseek(void) {
}
void _read(void) {
}
void _write(void) {
}

int main(void) {
  *(volatile uint32_t *)SYSMGR_FRZCTRL_VIOCTRL    = 0;
  *(volatile uint32_t *)SYSMGR_PINMUX_HPS_UART_RX = 0;
  *(volatile uint32_t *)SYSMGR_PINMUX_HPS_UART_TX = 0;
  *(volatile uint32_t *)SYSMGR_PINMUX_HPS_LED     = 0;
  *(volatile uint32_t *)SYSMGR_PINMUX_HPS_KEY     = 0;
  *(volatile uint32_t *)SYSMGR_SCANMGR_LOANIO1 |= (1 << (49 - 29));
  *(volatile uint32_t *)SYSMGR_SCANMGR_LOANIO1 |= (1 << (50 - 29));
  *(volatile uint32_t *)SYSMGR_SCANMGR_LOANIO1 |= (1 << (53 - 29));
  *(volatile uint32_t *)SYSMGR_SCANMGR_LOANIO1 |= (1 << (54 - 29));
  while (1);
  return 0;
}
```

- File > New > Source File
  - Source folder: `DE10_Standard_Beremetal`
  - Source file: `main.ld`

```ld
ENTRY(main)

MEMORY
{
    ram (rwx) : ORIGIN = 0x01000000, LENGTH = 64M
}

SECTIONS
{
    .text : {
        KEEP(*(.text.main))
        *(.text*)
        *(.rodata*)
    } > ram

    .data : {
        *(.data*)
    } > ram

    .bss : {
        *(.bss*)
    } > ram

    _stack_top = ORIGIN(ram) + LENGTH(ram);
}
```

## 8. Edit Settings

- Select `DE10_Standard_baremetal`
  - File > Properties > C/C++ Build > Settings
    - Target Processor
      - Arm family (-mcpu): `cortex-a9`
      - Architecture (-march): `armv7-a`
      - Instruction set: `Arm (-marm)`
    - GNU Arm Cross Create Flush Image > General
      - Output file format (-O): `Raw binary`
    - GNU Arm C Linker > General
      - Add `C:\Users\admin\Documents\AshlingWorkspace\DE10_Standard_Baremetal\main.ld`
    - GNU Arm C Linker > Miscellaneous
      - Confirm `Do not use syscalls (--specs=nosys.specs)` checked  
      - Other linker flags: `--static`

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
cp /mnt/c/Users/admin/Documents/CoDesign/soc_system.rbf mnt/
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
