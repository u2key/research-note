# RISC-V Tests

## 1. Clone riscv-tests
```
cd /opt
```
```
git clone https://github.com/riscv/riscv-tests
```
```
cd /opt/riscv-tests
```
```
git submodule update --init --recursive
```

## 2. Build RISC-V Tests
```
cd /opt/riscv-tests
```
```
autoconf
```
```
./configure --prefix=/opt/riscv/bin/riscv32-unknown-elf
```
```
make XLEN=32 ABI=ilp32 RISCV_MARCH=rv32imd vec_bmarks=""
```
