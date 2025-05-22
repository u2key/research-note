# Create Ubuntu Image for Intel DE1-SoC FPGA

## 1. Get Ubuntu Disk Image

- https://www.terasic.com.tw/cgi-bin/page/archive.pl?Language=English&No=836&PartNo=4

## 2. Prepare microSD card

- From attached file
  - Prerequisite: You need at least a 8GB microSD card.

## 3. Burn microSD card (with ubuntu)

- Search the microSD card device

```
sudo fdisk -l
```
```
Disk /dev/sdb: 3.64 GiB, 3904897024 bytes, 7626752 sectors
Disk model: Storage Device
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x000e90f2
```

- Format the microSD card

```
sudo parted /dev/sdb
```
```
(parted) mklabel gpt
```

- Burn the disk image to the microSD card

```
ff if=<Downloaded Disk Image Path> of=/dev/sdb status=progress && sync
```

## 4. Run Linux

- Insert the microSD card to DE1-SoC.
- Set MSEL on your DE1-SoC to 000000.
- Connect VGA monitor, USB mouse and keyboard to DE1-SoC.
  - Or use PuTTY to get console.
    - Connect DE1-SoC and PC using microUSB cable
    - Run PuTTY and set speed to 115200
- Power-on the board and you will see the Ubuntu Console.
- (Optional) Specify MAC Address

```
setenv ethaddr <Addr>
saveenv
reset
```

- (Optional) Resize root partition

```
parted
print all
resizepart 2
exit
df -H
resize2fs /dev/mmcblk0p2
```

