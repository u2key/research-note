# USRP (Universal Software Radio Peripheral) B210

> ## References
> - https://kb.ettus.com/B200/B210/B200mini/B205mini/B206mini_Getting_Started_Guides
> - https://www.ettus.com/all-products/ub210-kit/
> - https://www.ettus.com/wp-content/uploads/2019/01/b200-b210_spec_sheet.pdf
> - http://haljion.net/index.php?option=com_content&view=article&id=519:wsl-ise-webpack&catid=120:2019-11-18-02-29-10

## 1. Specifications

- Dual Channel Transceiver (70 MHz - 6 GHz)
- AD9361 RFIC direct conversion transceiver
  - Providing up to 56MHz of real-time bandwidth
- Spartan6 XC6SLX150 FPGA
- USRP Hardware Driver software
  - Developing with GNU Radio
  - Developing GSM base station with OpenBTS
  - Developing seamless transition code
- RF frontend
  - Analog Devices AD9361
  - Real time throughput benchmarked at 61.44MS/s quadrature
  - Streaming up to 56 MHz of real-time RF bandwidth

## 2. Install Xilinx ISE 14.7 VM Edition
### 2.1. Install Virtual Box 7.1.10
- https://download.virtualbox.org/virtualbox/7.1.10/VirtualBox-7.1.10-169112-Win.exe

### 2.2. Download Xilinx ISE Design Suite for Windows 10 and Windows 11 - 14.7 
- https://xilinx.com/downloadNav/vivado-design-tools/archive-ise.html

### 2.3. Disable Virtualization Check
```
notepad.exe C:\Users\admin\Downloads\Xilinx_ISE_14.7_Win10_14.7_VM_0213_1\bin\validate_virtualization_enabled.bat
```
```
@echo off
rem %SYSTEMROOT%\system32\windowspowershell\v1.0\powershell.exe -ExecutionPolicy Bypass -NoProfile -Command "& '%~dnp0.ps1'"
```

## 3. Install USRP Hardware Driver (UHD)
```
sudo apt install autoconf automake build-essential ccache cmake cpufrequtils doxygen ethtool g++ gcc git inetutils-tools libboost-all-dev libncurses5-dev libusb-1.0-0 libusb-1.0-0-dev libusb-dev python3-dev python3-mako python3-numpy python3-requests python3-scipy python3-setuptools python3-ruamel.yaml
```
```
cd
```
```
git clone https://github.com/EttusResearch/uhd.git
```
```
mkdir /home/ubuntu/uhd/host/build
```
```
cd /home/ubuntu/uhd/host/build
```
```
cmake -DCMAKE_INSTALL_PREFIX=/opt/uhd ../
```
```
-- Build type not specified: defaulting to release.
-- The CXX compiler identification is GNU 13.3.0
-- The C compiler identification is GNU 13.3.0
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
--
-- Configuring the Python interpreter...
-- Python interpreter: /usr/bin/python3 Version: 3.12.3
-- Override with: -DPYTHON_EXECUTABLE=<path-to-python>
-- Python runtime interpreter: /usr/bin/python3 Version: 3.12.3
-- Override with: -DRUNTIME_PYTHON_EXECUTABLE=<path-to-python>
-- Finding Python Libraries...
-- Python Libraries: /usr/lib/x86_64-linux-gnu/libpython3.12.so
-- Python include directories: /usr/include/python3.12;/usr/lib/python3/dist-packages/numpy/core/include
-- Operating on master branch.
fatal: no tag exactly matches '07a7a92ad6e09cc7e84aae5990aff563a4546e83'
-- Using UHD Images Directory: /opt/uhd/share/uhd/images
-- Performing Test HAVE_VISIBILITY_HIDDEN
-- Performing Test HAVE_VISIBILITY_HIDDEN - Success
-- Performing Test HAVE_VISIBILITY_INLINES_HIDDEN
-- Performing Test HAVE_VISIBILITY_INLINES_HIDDEN - Success
--
-- Configuring Boost C++ Libraries...
--
-- Checking for Boost version 1.66 or greater
--   Looking for required Boost components...
--   Enabling Boost Error Code Header Only
--   Checking whether std::string_view works in boost::asio
--     Enabling boost::asio use of std::string_view
--   Boost version: 1.83.0
--   Boost include directories: /usr/include
--   Boost library directories: /usr/lib/x86_64-linux-gnu
--   Boost libraries: Boost::chrono;Boost::date_time;Boost::filesystem;Boost::program_options;Boost::serialization;Boost::thread;Boost::unit_test_framework;Boost::system
-- Looking for Boost version 1.66 or greater - found
--
-- Python checking for compatible Python version
-- Python checking for compatible Python version - 3.12.3 satisfies minimum required version 3.7
--
-- Python checking for Mako templates module
-- Python checking for Mako templates module - 1.3.2 satisfies minimum required version 0.4.2
--
-- Python checking for requests module
-- Python checking for requests module - 2.31.0 satisfies minimum required version 2.0
--
-- Python checking for numpy module
-- Python checking for numpy module - 1.26.4 satisfies minimum required version 1.11
--
-- Python checking for ruamel.yaml module
-- Python checking for ruamel.yaml module - 0.17.21 satisfies minimum required version 0.15
--
-- Configuring LibUHD support...
--   Dependency Boost_FOUND = TRUE
--   Dependency HAVE_PYTHON_MODULE_MAKO = TRUE
--   Enabling LibUHD support.
--   Override with -DENABLE_LIBUHD=ON/OFF
--
-- Configuring LibUHD - C API support...
--   Dependency ENABLE_LIBUHD = ON
--   Enabling LibUHD - C API support.
--   Override with -DENABLE_C_API=ON/OFF
--
-- Configuring LibUHD - Python API support...
--   Dependency ENABLE_LIBUHD = ON
--   Dependency HAVE_PYTHON_MODULE_NUMPY = TRUE
--   Dependency HAVE_PYTHON_LIBS = TRUE
--   Dependency HAVE_PYTHON_PLAT_MIN_VERSION = TRUE
--   Enabling LibUHD - Python API support.
--   Override with -DENABLE_PYTHON_API=ON/OFF
--
-- Configuring Examples support...
--   Dependency ENABLE_LIBUHD = ON
--   Enabling Examples support.
--   Override with -DENABLE_EXAMPLES=ON/OFF
--
-- Configuring Utils support...
--   Dependency ENABLE_LIBUHD = ON
--   Enabling Utils support.
--   Override with -DENABLE_UTILS=ON/OFF
--
-- Configuring Tests support...
--   Dependency ENABLE_LIBUHD = ON
--   Enabling Tests support.
--   Override with -DENABLE_TESTS=ON/OFF
--
-- Configuring Python Module (Utils only) support...
--   Dependency HAVE_PYTHON_MODULE_NUMPY = TRUE
--   Dependency HAVE_PYTHON_MODULE_MAKO = TRUE
--   Dependency HAVE_PYTHON_MODULE_YAML = TRUE
--   Enabling Python Module (Utils only) support.
--   Override with -DENABLE_PYMOD_UTILS=ON/OFF
--
-- Configuring RFNoC/FPGA Development Files support...
--   Dependency ENABLE_LIBUHD = ON
--   Disabling RFNoC/FPGA Development Files support.
--   Override with -DENABLE_RFNOC_DEV=ON/OFF
--
-- Looking for libusb_handle_events_timeout_completed
-- Looking for libusb_handle_events_timeout_completed - found
-- Looking for libusb_error_name
-- Looking for libusb_error_name - found
-- Looking for libusb_strerror
-- Looking for libusb_strerror - found
-- Found LIBUSB: /usr/lib/x86_64-linux-gnu/libusb-1.0.so
-- Found PkgConfig: /usr/bin/pkg-config (found version "1.8.1")
-- Checking for module 'libdpdk>=18.11'
--   Package 'libdpdk', required by 'virtual:world', not found
-- Could NOT find DPDK (missing: DPDK_INCLUDE_DIRS DPDK_CFLAGS DPDK_LDFLAGS DPDK_LIBRARIES)
--
-- Configuring USB support...
--   Dependency ENABLE_LIBUHD = ON
--   Dependency LIBUSB_FOUND = TRUE
--   Enabling USB support.
--   Override with -DENABLE_USB=ON/OFF
--
-- Configuring B100 support...
--   Dependency ENABLE_LIBUHD = ON
--   Dependency ENABLE_USB = ON
--   Enabling B100 support.
--   Override with -DENABLE_B100=ON/OFF
--
-- Configuring B200 support...
--   Dependency ENABLE_LIBUHD = ON
--   Dependency ENABLE_USB = ON
--   Enabling B200 support.
--   Override with -DENABLE_B200=ON/OFF
--
-- Configuring USRP1 support...
--   Dependency ENABLE_LIBUHD = ON
--   Dependency ENABLE_USB = ON
--   Enabling USRP1 support.
--   Override with -DENABLE_USRP1=ON/OFF
--
-- Configuring USRP2 support...
--   Dependency ENABLE_LIBUHD = ON
--   Enabling USRP2 support.
--   Override with -DENABLE_USRP2=ON/OFF
--
-- Configuring X300 support...
--   Dependency ENABLE_LIBUHD = ON
--   Enabling X300 support.
--   Override with -DENABLE_X300=ON/OFF
--
-- Configuring MPMD support...
--   Dependency ENABLE_LIBUHD = ON
--   Enabling MPMD support.
--   Override with -DENABLE_MPMD=ON/OFF
--
-- Configuring SIM support...
--   Dependency ENABLE_LIBUHD = ON
--   Dependency ENABLE_MPMD = ON
--   Dependency ENABLE_PYTHON_API = ON
--   Enabling SIM support.
--   Override with -DENABLE_SIM=ON/OFF
--
-- Configuring N300 support...
--   Dependency ENABLE_LIBUHD = ON
--   Dependency ENABLE_MPMD = ON
--   Enabling N300 support.
--   Override with -DENABLE_N300=ON/OFF
--
-- Configuring N320 support...
--   Dependency ENABLE_LIBUHD = ON
--   Dependency ENABLE_MPMD = ON
--   Enabling N320 support.
--   Override with -DENABLE_N320=ON/OFF
--
-- Configuring E320 support...
--   Dependency ENABLE_LIBUHD = ON
--   Dependency ENABLE_MPMD = ON
--   Enabling E320 support.
--   Override with -DENABLE_E320=ON/OFF
--
-- Configuring E300 support...
--   Dependency ENABLE_LIBUHD = ON
--   Dependency ENABLE_MPMD = ON
--   Enabling E300 support.
--   Override with -DENABLE_E300=ON/OFF
--
-- Configuring X400 support...
--   Dependency ENABLE_LIBUHD = ON
--   Dependency ENABLE_MPMD = ON
--   Enabling X400 support.
--   Override with -DENABLE_X400=ON/OFF
--
-- Configuring OctoClock support...
--   Dependency ENABLE_LIBUHD = ON
--   Enabling OctoClock support.
--   Override with -DENABLE_OCTOCLOCK=ON/OFF
--
-- Configuring DPDK support...
--   Dependency ENABLE_MPMD = ON
--   Dependency DPDK_FOUND = FALSE
--   Disabling DPDK support.
--   Override with -DENABLE_DPDK=ON/OFF
--
-- Looking for C++ include emmintrin.h
-- Looking for C++ include emmintrin.h - found
-- Looking for C++ include arm_neon.h
-- Looking for C++ include arm_neon.h - not found
--
-- Configuring priority scheduling...
-- Performing Test HAVE_PTHREAD_SETSCHEDPARAM
-- Performing Test HAVE_PTHREAD_SETSCHEDPARAM - Success
-- Performing Test HAVE_WIN_SETTHREADPRIORITY
-- Performing Test HAVE_WIN_SETTHREADPRIORITY - Failed
--   Priority scheduling supported through pthread_setschedparam.
-- Performing Test HAVE_PTHREAD_SETNAME
-- Performing Test HAVE_PTHREAD_SETNAME - Success
--   Setting thread names is supported through pthread_setname_np.
-- Performing Test HAVE_PTHREAD_SETAFFINITY_NP
-- Performing Test HAVE_PTHREAD_SETAFFINITY_NP - Success
-- Performing Test HAVE_WIN_SETTHREADAFFINITYMASK
-- Performing Test HAVE_WIN_SETTHREADAFFINITYMASK - Failed
--   Setting thread affinity is supported through pthread_setaffinity_np.
--
-- Configuring module loading...
-- Performing Test HAVE_DLOPEN
-- Performing Test HAVE_DLOPEN - Success
-- Performing Test HAVE_LOAD_LIBRARY
-- Performing Test HAVE_LOAD_LIBRARY - Failed
--   Module loading supported through dlopen.
--
-- Configuring atomics support...
-- Performing Test HAVE_CXX_ATOMICS_WITHOUT_LIB
-- Performing Test HAVE_CXX_ATOMICS_WITHOUT_LIB - Success
-- Performing Test HAVE_CXX_ATOMICS64_WITHOUT_LIB
-- Performing Test HAVE_CXX_ATOMICS64_WITHOUT_LIB - Success
-- Performing Test HAVE_CXX_BOOST_ATOMICS_WITHOUT_LIB
-- Performing Test HAVE_CXX_BOOST_ATOMICS_WITHOUT_LIB - Success
--   Atomics support is built-in, no linking required.
--
-- Processing NI-RIO FPGA LVBITX Bitstreams...
--   Using x300.lvbitx_base for codegen
--   Using x310.lvbitx_base for codegen
--
-- USB support enabled via libusb.
--
-- Configuring interface address discovery...
-- Performing Test HAVE_GETIFADDRS
-- Performing Test HAVE_GETIFADDRS - Success
-- Looking for C++ include winsock2.h
-- Looking for C++ include winsock2.h - not found
--   Interface address discovery supported through getifaddrs.
-- Looking for C++ include atlbase.h
-- Looking for C++ include atlbase.h - not found
--
-- Loading build info.
-- Looking for wsyncup in /usr/lib/x86_64-linux-gnu/libcurses.so
-- Looking for wsyncup in /usr/lib/x86_64-linux-gnu/libcurses.so - found
-- Looking for cbreak in /usr/lib/x86_64-linux-gnu/libncurses.so
-- Looking for cbreak in /usr/lib/x86_64-linux-gnu/libncurses.so - found
-- Looking for nodelay in /usr/lib/x86_64-linux-gnu/libncurses.so
-- Looking for nodelay in /usr/lib/x86_64-linux-gnu/libncurses.so - found
-- Found Curses: /usr/lib/x86_64-linux-gnu/libncurses.so
-- Performing Test HAVE_C99_STRUCTDECL
-- Performing Test HAVE_C99_STRUCTDECL - Success
--
-- Adding B2XX device test target
-- Adding X3x0 device test target
-- Adding E3XX device test target
-- Adding N3XX device test target
-- Adding E32x device test target
-- Adding X410 device test target
-- Adding X440 device test target
--
-- Found Doxygen: /usr/bin/doxygen (found version "1.9.8") found components: doxygen missing components: dot
--
-- Configuring Manual support...
--   Dependency DOXYGEN_FOUND = YES
--   Enabling Manual support.
--   Override with -DENABLE_MANUAL=ON/OFF
--
-- Configuring API/Doxygen support...
--   Dependency DOXYGEN_FOUND = YES
--   Enabling API/Doxygen support.
--   Override with -DENABLE_DOXYGEN=ON/OFF
--
-- Found GZip: /usr/bin/gzip
--
-- Compressed Man Pages enabled
--   Override with -DENABLE_MAN_PAGE_COMPRESSION=ON/OFF
--
-- Configuring Man Pages support...
--   Dependency NOT_WIN32 = TRUE
--   Dependency GZIP_FOUND = TRUE
--   Enabling Man Pages support.
--   Override with -DENABLE_MAN_PAGES=ON/OFF
-- Using in-tree Pybind11.
--
-- Python checking for gevent module
-- Python checking for gevent module - "import gevent" failed (is it installed?)
--
-- Python checking for mprpc module
-- Python checking for mprpc module - "import mprpc" failed (is it installed?)
--
-- Python checking for pyudev module
-- Python checking for pyudev module - "import pyudev" failed (is it installed?)
--
-- Python checking for pyroute2 module
-- Python checking for pyroute2 module - "import pyroute2" failed (is it installed?)
-- MPM unit test Python package prerequisites not met; skipping
--
--
-- Python checking for virtual environment
-- Python checking for virtual environment - "assert sys.prefix != sys.base_prefix" failed
-- Installing 'uhd' Python module to: /opt/uhd/lib/python3.12/site-packages
--
-- ######################################################
-- # UHD enabled components
-- ######################################################
--   * LibUHD
--   * LibUHD - C API
--   * LibUHD - Python API
--   * Examples
--   * Utils
--   * Tests
--   * Python Module (Utils only)
--   * USB
--   * B100
--   * B200
--   * USRP1
--   * USRP2
--   * X300
--   * MPMD
--   * SIM
--   * N300
--   * N320
--   * E320
--   * E300
--   * X400
--   * OctoClock
--   * Manual
--   * API/Doxygen
--   * Man Pages
--
-- ######################################################
-- # UHD disabled components
-- ######################################################
--   * RFNoC/FPGA Development Files
--   * DPDK
--
-- ******************************************************
-- * You are building the UHD development master branch.
-- * For production code, we recommend our stable,
-- * releases or using the release branch (maint).
-- ******************************************************
-- Building version: 4.9.0.0-2-g07a7a92a
-- Using install prefix: /opt/uhd
-- Configuring done (6.1s)
-- Generating done (0.4s)
-- Build files have been written to: /home/ubuntu/uhd/host/build
```
```
make
```
```
[  0%] Building CXX object lib/deps/rpclib/CMakeFiles/uhd_rpclib.dir/lib/rpc/dispatcher.cc.o
[  0%] Building CXX object lib/deps/rpclib/CMakeFiles/uhd_rpclib.dir/lib/rpc/server.cc.o
[  0%] Building CXX object lib/deps/rpclib/CMakeFiles/uhd_rpclib.dir/lib/rpc/client.cc.o
...
[100%] Built target pyuhd
[100%] Generating build/timestamp
[100%] Built target usrp_mpm
[100%] Built target copy_mpm_packages
[100%] Generating build/timestamp
Including packages in pyuhd: ['usrp_mpm', 'uhd', 'usrp_mpm.xports', 'usrp_mpm.simulator', 'usrp_mpm.dboard_manager', 'usrp_mpm.sys_utils', 'usrp_mpm.periph_manager', 'uhd.usrp', 'uhd.usrp_clock', 'uhd.usrpctl', 'uhd.rfnoc_utils', 'uhd.dsp', 'uhd.utils', 'uhd.usrp.chips', 'uhd.usrp.cal', 'uhd.usrpctl.commands', 'uhd.rfnoc_utils.templates', 'uhd.rfnoc_utils.templates.modules', 'uhd.rfnoc_utils.modtool_commands']
[100%] Built target pyuhd_library
```
```
make test
```
```
Running tests...
Test project /home/ubuntu/uhd/host/build
      Start  1: actions_test
 1/98 Test  #1: actions_test .....................   Passed    0.15 sec
...
      Start 98: custom_reg_test
98/98 Test #98: custom_reg_test ..................   Passed    0.02 sec

100% tests passed, 0 tests failed out of 98

Total Test time (real) =  20.36 sec
```
```
sudo make install
```
```
[  1%] Built target uhd_rpclib
...
[100%] Built target pyuhd_library
Install the project...
-- Install configuration: "Release"
...
-- Installing: /opt/uhd/bin/usrp_hwd.py
```
```
sudo ldconfig
```
```
echo 'PATH="${PATH}:/opt/uhd/bin/"' >> ${HOME}/.bashrc
```

## 4. Operating in Loopback Configuration
### 4.1. Finding the Device
```
uhd_usrp_probe
```

