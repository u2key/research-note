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

### 2.3. OpenCR Pins
```c
#define BDPIN_UART1_RX     80
#define BDPIN_UART1_RX     81
#define BDPIN_UART2_RX     82
#define BDPIN_UART2_RX     83
#define BDPIN_UART6_RX      0
#define BDPIN_UART6_TX      1

#define BDPIN_EXTI_0        2
#define BDPIN_EXTI_1        3
#define BDPIN_EXTI_2        4
#define BDPIN_EXTI_3        7
#define BDPIN_EXTI_4        8
#define BDPIN_EXTI_5       42
#define BDPIN_EXTI_6       45
#define BDPIN_EXTI_7       72
#define BDPIN_EXTI_8       75

#define BDPIN_TIM1_CH1      5
#define BDPIN_TIM2_CH3      6
#define BDPIN_TIM3_CH1      3
#define BDPIN_TIM9_CH2      9
#define BDPIN_TIM11_CH1    10
#define BDPIN_TIM12_CH2    11

#define BDPIN_SPI_CS_IMU   28
#define BDPIN_SPI_CLK_IMU  37
#define BDPIN_SPI_SDO_IMU  38
#define BDPIN_SPI_SDI_IMU  39
#define BDPIN_SPI2_NSS     10
#define BDPIN_SPI2_MOSI    11
#define BDPIN_SPI2_MISO    12
#define BDPIN_SPI2_SCK     13

#define BDPIN_I2C1_SDA     14
#define BDPIN_I2C1_SCL     15

#define BDPIN_A0           16
#define BDPIN_A1           17
#define BDPIN_A2           18
#define BDPIN_A3           19
#define BDPIN_A4           20
#define BDPIN_A5           21

#define BDPIN_LED_BUILTIN  13
#define BDPIN_LED_USER_1   22
#define BDPIN_LED_USER_2   23
#define BDPIN_LED_USER_3   24
#define BDPIN_LED_USER_4   25
#define BDPIN_LED_STATUS   36

#define BDPIN_DIP_SW_1     26
#define BDPIN_DIP_SW_2     27

#define BDPIN_BAT_PWR_ADC  29

#define BDPIN_BUZZER       31

#define BDPIN_DXL_PWR_EN   32

#define BDPIN_PUSH_SW_1    34
#define BDPIN_PUSH_SW_2    35

#define BDPIN_OLLO_P1_SIG1 40
#define BDPIN_OLLO_P1_SIG2 41
#define BDPIN_OLLO_P1_ADC  42
#define BDPIN_OLLO_P2_SIG1 43
#define BDPIN_OLLO_P2_SIG2 44
#define BDPIN_OLLO_P2_ADC  45
#define BDPIN_OLLO_P3_SIG1 70
#define BDPIN_OLLO_P3_SIG2 71
#define BDPIN_OLLO_P3_ADC  72
#define BDPIN_OLLO_P4_SIG1 73
#define BDPIN_OLLO_P4_SIG2 74
#define BDPIN_OLLO_P4_ADC  75
#define BDPIN_OLLO_SLEEP   46

#define BDPIN_GPIO_1       50
#define BDPIN_GPIO_2       51
#define BDPIN_GPIO_3       52
#define BDPIN_GPIO_4       53
#define BDPIN_GPIO_5       54
#define BDPIN_GPIO_6       55
#define BDPIN_GPIO_7       56
#define BDPIN_GPIO_8       57
#define BDPIN_GPIO_9       58
#define BDPIN_GPIO_10      59
#define BDPIN_GPIO_11      60
#define BDPIN_GPIO_12      61
#define BDPIN_GPIO_13      62
#define BDPIN_GPIO_14      63
#define BDPIN_GPIO_15      64
#define BDPIN_GPIO_16      65
#define BDPIN_GPIO_17      66
#define BDPIN_GPIO_18      67
```

## 3. Zybo Z7 FPGA Image Recognition Development

> ### References
> - https://fumimaker.net/entry/2020/02/06/002934
> - https://phys-higashi.com/73/#toc8
> - https://marsee101.blog.fc2.com/blog-entry-4027.html

### 1.1. Install Xilinx Vivado
