# Autonomous Vehicle

## 1. Install Developing Tools for ZYBO Z7
### 1.1. Install Vivado Design Suite - HLx Editions Update 3 - 2019.1
- https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vivado-design-tools/archive.html

## 2. Prepare for OpenCR Development

> ### References
> - https://emanual.robotis.com/docs/en/parts/controller/opencr10_jp/
> - https://qiita.com/basalte/items/e28d60ce0681c69ccee7

### 2.1. Import UDEV Rule
```
wget https://raw.githubusercontent.com/ROBOTIS-GIT/OpenCR/master/99-opencr-cdc.rules
```
```
sudo cp ./99-opencr-cdc.rules /etc/udev/rules.d/
```
```
sudo udevadm control --reload-rules
```
```
sudo udevadm trigger
```

### 2.2. Install Arduino IDE
```
sudo chmod 777 /opt
```
```
mkdir /opt/arduino-ide
```
```
cd /opt/arduino-ide
```
```
wget https://downloads.arduino.cc/arduino-ide/arduino-ide_2.3.6_Linux_64bit.zip
```
```
unzip arduino-ide_2.3.6_Linux_64bit.zip
```
```
sudo dpkg --add-architecture i386
```
```
sudo apt update && sudo apt install libasound2t64 libncurses5-dev libncurses5-dev:i386 libnss3-dev
```
```
/opt/arduino-ide/arduino-ide_2.3.6_Linux_64bit/arduino-ide
```

- Open `File` > `Preferences`
  - Edit `Additional boards manager URLs`
    - `https://raw.githubusercontent.com/ROBOTIS-GIT/OpenCR/master/arduino/opencr_release/package_opencr_index.json`

- Open `Tools` > `Board` > `Board Manager...`
  - Type `OpenCR` in `Filter your search` box
  - Inatall `OpenCR by ROBOTIS` 

## 3. Zybo Z7 FPGA Image Recognition Development

> ### References
> - https://fumimaker.net/entry/2020/02/06/002934
> - https://phys-higashi.com/73/#toc8
> - https://marsee101.blog.fc2.com/blog-entry-4027.html

### 1.1. Install Xilinx Vivado
