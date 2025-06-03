# Generate RBF (Raw Binary File) 

## 1. The Reason why we generate RBF 

- Sometime, after FPGA configuration using JTAG UART, HPS loses control.
- In this case, you should make RBF file.

## 2. How to make RBF

1. Open Quartus Project
2. `File` > `Convert Programming Files`
3. Set `Programming File Type` to `Raw Binary File (.rbf)`
4. Set `File Name` to `soc_system.rbd`
5. `Input files to convert` > `SoF Data` > `Add File`

## 3. Copy RBF to U-BOOT partition
