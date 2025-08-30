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

## 2. Modify the Source File
```
diff /opt/riscv-tests/env/p/link.ld /opt/riscv-tests/env/p/link.ld.original
```
```
6c6
<   . = 0x00000000;
---
>   . = 0x80000000;
```

## 3. To Bypass Atomic Operation, Pseudo Operation Must Be Inserted
```
vim benchmarks/common/syscalls.c
```
```
// Software Pseudo Atomic Operation for Single Thread Processor
unsigned int __sync_fetch_and_add_4(volatile void *ptr, unsigned int val) {
  unsigned int *uint_ptr = (unsigned int *)ptr;
  unsigned int old = *uint_ptr;
  *uint_ptr += val;
  return old;
}
```

## 4. Build RISC-V Tests
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
