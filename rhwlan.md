# Radiation Hardened Wireless LAN

## 1. Open IEEE 802.11 Sample Project of MATLAB / Simulink
### 1.1. Import WLAN HDL Transmitter Example
```
openExample('whdl/WLANHDLTransmitterExample', 'workDir', '~/WLANHDLTransmitter')
```

### 1.2. Import WLAN HDL Receiver Example
```
openExample('whdl/WLANHDLReceiverExample', 'workDir', '~/WLANHDLReceiver')
```

## 2. Project Parameters for IEEE 802.11n Compatibility

<div align="center">
  <table>
    <tr><td>Parameter Name</td><td>Value</td></tr>
    <tr><td></td><td></td></tr>
  </table>
</div>

## 3. Generate Hardware Description Language (HDL)
### 3.1. Run the Project on MATLAB

### 3.2. Click `Generate HDL Code` on Simulink Top-Level
<div align="center"><img src="imgs/matlab-open-simulink-top-level-file.png" width="500"></div>

## 4. The Generated HDL are Implemented into RF-SoC
### 4.1. IEEE 802.11n Specifications

<div align="center">
  <table>
    <tr><td>Item</td><td>Value</td></tr>
    <tr><td>Required Clock Freq.</td><td>160 MHz</td></tr>
    <tr><td>ADC Type</td><td>SAR ADC</td></tr>
  </table>
</div>
