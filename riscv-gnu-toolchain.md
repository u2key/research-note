# RISCV GNU Toolchain

## OS Information
```
cat /etc/os-release
```
```
PRETTY_NAME="Ubuntu 24.04.2 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04.2 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo
```

## 1. Install Required Packages
```
sudo apt update && sudo apt upgrade && sudo apt install autoconf automake autotools-dev bc bison build-essential curl flex gawk gcc git gperf libexpat-dev libgmp-dev libmpc-dev libmpfr-dev libtool make patchutils python3 texinfo vim zlib1g-dev
```

## 2. Clone riscv-gnu-toolchain
```
sudo chmod 777 /opt
```
```
cd /opt
```
```
git clone https://github.com/riscv/riscv-gnu-toolchain.git
```

## 3. Build the Toolchain
```
mkdir /opt/riscv
```
```
cd /opt/riscv-gnu-toolchain/
```
```
./configure --prefix=/opt/riscv --with-arch=rv32ima --with-abi=ilp32 --enable-multilib
```
```
make -j 8 newlib
```
```
echo 'export PATH="{$PATH}:/opt/riscv/bin"' >> ~/.bashrc
```
