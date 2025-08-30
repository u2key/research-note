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

## 5. Convert ELF to Instruction Memory Format
```
import os
import sys

instruction_bits = 32
instruction_bytes = int(instruction_bits / 8)

if len(sys.argv) < 2:
  print(f"python3 imem.py <ELF Path>")
  exit()

elf_path = sys.argv[1]
os.system(f"riscv32-unknown-elf-objcopy -O binary {elf_path} {elf_path}.bin")
with open(f"{elf_path}.bin", "rb") as f:
  instruction_hex = [f"{hex:02x}" for hex in f.read()]
instruction_hex += ["00"] * (instruction_bytes - int(len(instruction_hex) % instruction_bytes))
instruction_nop = "".join(["0" for _ in range(instruction_bytes)])

with open(f"{elf_path}_imem.vhdl", "w") as f:
  f.write(f"LIBRARY IEEE;\n")
  f.write(f"USE IEEE.STD_LOGIC_1164.ALL;\n")
  f.write(f"PACKAGE IMEM IS\n")
  f.write(f"  TYPE IMEM_TYPE IS ARRAY (0 TO 1023) of STD_LOGIC_VECTOR({instruction_bits-1} DOWNTO 0);\n")
  f.write(f"  CONSTANT DATA : IMEM_TYPE := (\n")
  for n, a in enumerate(range(0, len(instruction_hex), instruction_bytes)):
    instruction = "".join([instruction_hex[a + instruction_bytes - b] for b in range(1, instruction_bytes+1, 1)])
    f.write(f"    ({n:%3d} => X\"{instruction}\"),\n")
  f.write(f"    (OTHERS => X\"{instruction_nop}\")\n")
  f.write(f"  );\n")
  f.write(f"END PACKAGE;\n")
    
```
