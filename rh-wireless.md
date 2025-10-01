# Radiation-Hardened Wireless Communication System

## 1. Generate Transmitter Hardware Description Language (HDL)
### 1.1. Open a Sample Project
```
openExample('whdl/WLANHDLTransmitterExample', 'workDir', 'C:\Users\admin\Documents\rh-wireless\transmitter')
```

### 1.2. Modify `runWLANTransmitter.m`
```
% The |runWLANTransmitter| script demonstrates the |wlanhdlTransmitter| Simulink(R) model by
%  performing these steps:
%  1. Set WLAN transmitter parameter configuration.
%  2. Generate input for the model.
%  3. Compare Simulink output with MATLAB |wlanWaveformGenerator| function output.

%   Copyright 2024 The MathWorks, Inc.

%% WLAN HDL Transmitter Input Data Generation
numOfPackets = 1; % number of packets 
frameFormatIndex = 1; % 0 --> 'Non-HT', 1 --> 'HT-MF', 2 --> 'VHT'
MCS = 7; % 0 to 7
...
```

### 1.3. Run the Project
```
WLANHDLTransmitterExample
```

### 1.4. HDL Coder Settings
- HDL Code Generation
  - Generate HDL: `wlanhdlTransmitter/wlanHDLTx`
  - Target > Work Flow: `Generic ASIC/FPGA`
  - Synthesis Tool: `Xilinx ISE`
  - Family: `Spartan6`
  - Device: `xc6slx150`
  - Package: `fgg484`
  - Target Frequency (MHz): `300`

### 1.5. Generate HDL Code

### 1.6. Generate Test Bench

## 2. Receiver Side
### 2.1. Open a Sample Project
```
openExample('whdl/WLANHDLReceiverExample', 'workDir', 'C:\Users\admin\Documents\rh-wireless\receiver')
```

### 2.2. Modify ``
